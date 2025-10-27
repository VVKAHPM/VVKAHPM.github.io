Understanding the Mechanism of   LLMs  

Lecturer: Liangming Pan, Zijun Yao, Jiaran Ye

#### Part 1. An Introduction to Science of LLM

"长思考"推理模型
对其机理认识不足——黑盒

Open questions:
- How is knowledge stored and retrieved in LLMs?
- How do models perform complex reasoning internally?
- Why does simple next-token prediction training lead to emergent abilities?
- What benefits do RL training bring to the LLMs?

>LLMs represent the first "intelligent agents" that We can fully observe and manipulate

**Philosophy**——Bottom-up (Reductionism) / Top-Down(Systems theory)
**Methodology**——Reverse Engineering(根据大模型的储存得到它表示的概念) / Concept Based Interpretability(给定概念，寻找大模型是如何储存这种概念的)
**Techniques**——Neuron Attribution Circuit Finding Sparse Autoencoder... / Concept Probing Synthesizing Data Behavioral Science...

类比人脑研究——大脑对应...，小脑对应控制...，寻找大模型每个部分的对应

**Example of Reverse Engineering**

Specific Component -> Interpret weights / activations

1. identify features
2. describe process

![[90651086b80ab886f5d21bb9fa70cc3d.jpg]]

**Polysemanticity Porblem**
> There are far more features in the world than MLP neurons
	-> **Superposition:** models must represent more concepts than they have neurons

**Sparse Auto-Encoder**
>**Key idea:** decompose model activations into sparsely active components("features"), which turn out in many cases to correspond to human-interpretable concepts.

**Cross-Layer Transcoder**
> **Key idea:** Building an Interpretable Replacement Model

**An example of multi-step reasoning**
Dallas->Texas->Austin
通过 feature 发现大模型的推理过程

**Limitation**
- SAE reconstruction errors are too high
- An approximate explanation is not a true explanation.
- SAE method are expensive to apply to large models
- The features we find might not be the features we want

**Dilemma:** We are trying to explain a black box and then to interpret it
**Quesition**: Why not build a "glass-box" at first?


#### Part 2. Sparse Auto-Encoder (SAE)

找到模型完成一个功能的最小单元是什么——(细胞)

**自然语言对特征的描述是稀疏的**，我们不会描述一个物件的全部特征。导致训练出来的模型具有严重的多重语义性。一个 Neuro 需要表示多个特征

**How to resolve superposition**


**Dictionary Learning as Sparse Autoencoder**

LLM ->(Train an SAE) SAE -> (Model Reconstruction) LLM

要求解码后的 LLM 和编码前的 LLM identical
$$
	z=SparseConstraint(W_{enc}(x))
$$

什么样的大模型是更好的：
**Sacling Law of LLMs**: Model Size + Training Data -> Training FLOPs （浮点操作数）-> Model Performance
观察计算量与最后 Loss 之间的曲线关系

**What Can SAE DO:**
 Assessing Interpretability of Features Extracted by SAEs
 
 
 > **Applications:** SAE interprets Linguistic Features
 


**Model Steering with SAES**
- Causality is the essence of any kind of science
- Mechanistic Interpretability aims to build the science of LLM
找到相关性-> 通过干预实验证明因果性
**SAEs Are Good for Steering - If You Select the Right Features**
- Select features with steering objectives

**Concerns about SAEs**
- Faithfulness
- Too much computation
- Too much storage

**Furture Directions of OpenSAE**
- Automatic Feature Interpretation
- Further Sacling up OpenSAE with More Training Tokens and Larger Model Sacle
- Post-Train Sparse Autoencoder for Better Steering


#### Part 3. How does Transformer Learn Implicit Reasoning

![[14db45ca97010a631a81900c04ca7875.jpg]]
模型做多跳推理的结果并不与我们设想一致——对首相知识进行编辑，但是询问首相的妻子回答仍然一样。

> It's hard to tell whether the model is reasoning - or just taking shortcuts.

**Unverified premise 1:** Model executes reasoning by combinating single knowledges.
**Unverified premise 2:** Decodability reflects the model's internal reasoning.

**Controlled Environment for our Analysis**
- Atomic Triples
  (e1, r1) -> e2
- 2-hop Queries
  (e1, r1, r2) -> e3

![[71080294dc27c13bd5ef66927551469f.jpg]]

**Finding: ID triples are not necessary for ID generalization**

