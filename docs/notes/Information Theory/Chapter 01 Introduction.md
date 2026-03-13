---
tags:
  - InformationTheory
  - Lecture
date: 2026-03-03
---
## Two purposes

1. Quantify Info: **Entropy**
2. Communication: **rate**
    - Noisy channel, maximal rate?
    - Error Correcting Code

## 1. 什么是 communication

```mermaid
flowchart LR
    A([发送方]) -- 媒介 --> B([接收方])
```

- 依照事先约定好的 protocol

## 2. 信息论的诞生 —— AT&T Bell Lab

- [[Claude Shannon]]
- *A Mathematical Theory of Communication*, 1948

## 3. 为什么 AI 需要信息论？

> [!abstract] Core Connection
> 现代 AI 的训练过程本质上是**信息压缩**与**分布匹配**的过程。

### 1. 损失函数 (Loss Function)
- **[[Cross Entropy]] (交叉熵)**: 几乎所有分类任务（Classification）和**大语言模型（LLM）**的训练目标。
  - Minimizing Loss $\iff$ Minimizing Perplexity $\iff$ Minimizing Entropy of the error distribution.

### 2. 生成模型 (Generative Models)
- **[[KL Divergence]] (相对熵)**: 衡量两个分布的差异。
  - **VAE**: 约束 Latent Space 分布。
  - **RLHF (Reinforcement Learning from Human Feedback)**: 确保微调后的模型（Policy）不会偏离基座模型（Reference Model）太远 (e.g., in PPO algorithm).

### 3. 深度学习理论
- **[[Information Bottleneck]]**: 深度神经网络是在寻找一种 Trade-off：
  - 压缩输入 $X$ 的信息（只保留相关特征）。
  - 最大化关于输出 $Y$ 的预测能力。
