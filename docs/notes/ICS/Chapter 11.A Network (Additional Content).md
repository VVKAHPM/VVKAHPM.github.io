## 1. What is the Internet? What is a protocol?

Q1. What's included in a society?

- People, entities -> internet components
- Services -> internet services
- Laws and rules -> protocols, standards

### 1.1 The Internet: a "nuts and bolts" view

Billions of connected computing **devices**:

- **hosts** (主机) = end systems
- running network apps at Internet's "edge"

**Packet switches** (数据包交换机): forward packets (chunks of data)
- routers (路由器), switches (交换机)

**Communication links** 

- fiber, copper, radio, satellite
- transmission rate: bandwidth (带宽)

**Networks**

- collection of devices, routers, links: managed by an organization

![](Pasted%20image%2020251211151936.png)


**Internet: "network of networks"**

- Interconnected ISPs

**Protocols** are everywhere

- control sending, receiving of messages
- e.g., HTTP(Web), streaming video, Skype(网络通话), TCP, IP, WiFi, 4/5G, Ethernet

**Internet standards**

- RFC: Request for Comments
- IETF: Internet Engineering Task Force

![](Pasted%20image%2020251211152243.png)


### 1.2 The Internet: a "services" view
![](Pasted%20image%2020251211152354.png)

### 1.3 Protocol
> Protocols define the format, order of messages sent and received among network entities, and actions taken on message transmission, receipt.


## 2. Network edge: hosts, access network, physical media
### 2.1 A closer look at Internet structure

#### 1. 网络边缘 (Network edge)

- **定义：** 数据的**起点和终点**，是连接到互联网末端的设备。
    
- **组成：** 包括 **主机 (hosts)**，即 **客户端 (clients)**（如您的笔记本电脑、手机）和 **服务器 (servers)**（通常在数据中心）。
    
- **作用：** 产生或接收数据。
    

#### 2. 接入网络和物理媒体 (Access networks, physical media)

- **定义：** 允许网络边缘设备（主机）连接到互联网核心的**物理链路**。
    
- **组成：** 包括**有线 (wired)** 和**无线 (wireless)** 通信链路。
    
- **作用：** 将边缘设备连接到它们最近的路由器，是数据进入互联网的第一步。
    

#### 3. 网络核心 (Network core)

- **定义：** 互联网的**骨干网络**，负责在不同网络之间转发数据。
    
- **组成：** 由**互连的路由器 (interconnected routers)** 构成，通常被称为一个**网络的网络 (network of networks)**。
    
- **作用：** 接收来自接入网络的数据，并通过路由器的路径，将数据包从源头转发到目的地。核心网络主要由不同层级的 **ISP (Internet Service Provider)** 构成（如图中的 Local/Regional ISP 和 National/Global ISP）。

## 3. Protocol layers
![](Pasted%20image%2020251211154041.png)
![](Pasted%20image%2020251211154539.png)

|**编号**|**TCP/IP 五层名称 (中文)**|**对应 OSI 层级**|**主要协议及功能**|
|---|---|---|---|
|**5**|**应用层**|5, 6, 7|包含 OSI 的会话、表示、应用层功能 (HTTP, DNS, SMTP)。|
|**4**|**传输层**|4|提供端到端的通信和连接管理 (TCP, UDP)。|
|**3**|**网络层**|3|负责数据包的路由和逻辑寻址 (IP)。|
|**2**|**链路层**|2|负责帧的传输和物理寻址 (Ethernet, Wi-Fi, PPP)。|
|**1**|**物理层**|1|负责比特流的物理传输。|
