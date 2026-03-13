---
tags:
  - InformationTheory
  - Lecture
date: 2026-03-13
---

## Overview

> [!abstract] Central Question
> Given a source that emits symbols according to a known probability distribution, what is the **minimum expected number of bits** required to represent each symbol? The answer — derived from first principles — is the **entropy** of the distribution.

---

## 1. The Binary Alphabet

Shannon's framework encodes information using the binary alphabet $\Sigma = \{0, 1\}$. A **codeword** is any finite binary string $c \in \{0,1\}^*$, and its **length** is the number of bits it contains.

**A motivating example.** Suppose we encode three symbols as:

$$W \to 0, \quad L \to 1, \quad T \to 01$$

- Transmitting a *single* symbol is unambiguous: receiving `0` can only mean $W$.
- But over an *infinite* stream, ambiguity arises: the string `01` could be parsed as either $(W, L)$ or $(T)$.

This example illustrates that a code must be **uniquely decodable** — every finite sequence of codewords must have exactly one valid parse, regardless of how many symbols are transmitted.

> [!tip] Unique Decodability as Injectivity
> It is useful to think of a code as a concatenation function:
>
> $$f\bigl((x_{i_1}, \ldots, x_{i_k})\bigr) = c_{i_1} c_{i_2} \cdots c_{i_k} \in \{0,1\}^*$$
>
> A code is **uniquely decodable** if and only if $f$ is **injective** over sequences of *all* lengths — no two distinct symbol sequences map to the same binary string.
>
> This is strictly stronger than requiring $c_i \neq c_j$ for $i \neq j$ (called a **non-singular** code). The W/L/T example is non-singular ($0$, $1$, and $01$ are all distinct), yet fails unique decodability because $f(W,L) = f(T) = \texttt{01}$.
>
> Prefix-free codes go one step further: they are **online** (or *instantaneously*) decodable — each symbol can be identified the moment its final bit arrives, with no look-ahead required.

---

## 2. Formal Model of a Source

A discrete **information source** is modeled as a random variable

$$X \sim P = (p_1, p_2, \ldots, p_n)$$

where $x_i$ is the $i$-th message and $p_i = \Pr[X = x_i]$.

A **binary source code** is a mapping $\varphi: \{x_1, \ldots, x_n\} \to \{0,1\}^*$, assigning each message a binary codeword $c_i = \varphi(x_i)$ of length $\ell_i = |c_i|$.

The **expected code length** is:

$$\bar{\ell} := \sum_{i=1}^{n} p_i \ell_i$$

**Goal:** find code lengths $\ell_1, \ldots, \ell_n$ that minimize $\bar{\ell}$, subject to the code remaining uniquely decodable.

---

## 3. Prefix-Free Codes and the Kraft Inequality

### 3.1 Prefix-Free Codes

A code is **prefix-free** (also called a *prefix code*) if no codeword is a proper prefix of another. Prefix-free codes are naturally represented as **binary trees**: each codeword corresponds to a leaf, and decoding requires no look-ahead — a symbol can be decoded as soon as its leaf is reached.

> [!tip] Why Prefix-Free Codes Are Sufficient
> Every uniquely decodable code can be replaced by a prefix-free code **with identical code lengths**. Since we only care about minimizing $\bar{\ell} = \sum p_i \ell_i$, and the lengths are unchanged, we may restrict our attention to prefix-free codes without any loss of generality.
>
> This equivalence is the content of **McMillan's Theorem** below.

### 3.2 The Kraft Inequality

> [!note] Lemma (Kraft Inequality)
> Let $C = (c_1, \ldots, c_n)$ be a **prefix-free** code with lengths $\ell_i = |c_i|$. Then:
>
> $$\sum_{i=1}^{n} 2^{-\ell_i} \leq 1$$
>
> Moreover, for any **optimal** prefix-free code, **equality holds**.

> [!question]- Proof (Kraft Inequality)
> Consider the full binary tree of depth $\ell_{\max} = \max_i \ell_i$. Each codeword $c_i$ "claims" all $2^{\ell_{\max} - \ell_i}$ leaves that are its descendants (including itself if it is already a leaf at depth $\ell_i$).
>
> Since the code is prefix-free, no two codewords share a descendant leaf. The total number of leaves claimed is therefore:
>
> $$\sum_{i=1}^{n} 2^{\ell_{\max} - \ell_i} \leq 2^{\ell_{\max}}$$
>
> Dividing both sides by $2^{\ell_{\max}}$ yields $\sum_{i=1}^n 2^{-\ell_i} \leq 1$. $\square$

### 3.3 McMillan's Theorem

The Kraft inequality holds not only for prefix-free codes, but for *all* uniquely decodable codes. This is the deep result that justifies focusing on prefix codes.

> [!note] Theorem (McMillan)
> If $C = (c_1, \ldots, c_n)$ is a **uniquely decodable** code with lengths $\ell_1, \ldots, \ell_n$, then:
>
> $$\sum_{i=1}^{n} 2^{-\ell_i} \leq 1$$

> [!question]- Proof (McMillan's Theorem)
> Let $K = \sum_{i=1}^n 2^{-\ell_i}$ and $\ell_{\max} = \max_i \ell_i$. Raise $K$ to the $k$-th power:
>
> $$K^k = \left(\sum_{i=1}^{n} 2^{-\ell_i}\right)^k = \sum_{(i_1, \ldots, i_k) \in [n]^k} 2^{-(\ell_{i_1} + \cdots + \ell_{i_k})}$$
>
> Grouping terms by total length $m = \ell_{i_1} + \cdots + \ell_{i_k}$:
>
> $$K^k = \sum_{m=k}^{k\,\ell_{\max}} A_k(m)\cdot 2^{-m}$$
>
> where $A_k(m)$ is the number of $k$-tuples with codewords of total length $m$.
>
> **Bounding $A_k(m)$.** The map $g: (i_1, \ldots, i_k) \mapsto c_{i_1} \cdots c_{i_k}$ restricted to total-length-$m$ tuples is injective by unique decodability: two distinct $k$-tuples yielding the same binary string would give two parses of that string, a contradiction. Since $|\{0,1\}^m| = 2^m$, we have $A_k(m) \leq 2^m$.
>
> Substituting:
>
> $$K^k \leq \sum_{m=k}^{k\,\ell_{\max}} 2^m \cdot 2^{-m} = k(\ell_{\max} - 1) + 1 \leq k\,\ell_{\max}$$
>
> Therefore $K \leq (k\,\ell_{\max})^{1/k}$ for all $k \geq 1$. Taking $k \to \infty$:
>
> $$K \leq \lim_{k \to \infty} (k\,\ell_{\max})^{1/k} = 1$$
>
> since $k^{1/k} \to 1$ and $\ell_{\max}^{1/k} \to 1$. Therefore $K \leq 1$. $\square$
>
> > [!note] Insight: Why raise to the $k$-th power?
> > The expansion $K^k = \sum_{(i_1,\ldots,i_k)} 2^{-(\ell_{i_1}+\cdots+\ell_{i_k})}$ is exactly the **Kraft sum of the $k$-fold block code** — the code that encodes length-$k$ symbol sequences by concatenating their codewords. Raising to the $k$-th power is equivalent to asking: "what does this code look like when used to transmit $k$ symbols at once?"
> >
> > The proof is an **amplification argument**. If $K > 1$, then $K^k$ grows exponentially in $k$. But unique decodability of the block code forces $K^k \leq k\,\ell_{\max}$, which is only polynomial. Taking $k \to \infty$ makes this contradiction unavoidable — violations invisible at the single-symbol level are amplified until they cannot be sustained.

> [!important] Corollary
> For any uniquely decodable code $C$, there exists a prefix-free code $C'$ with $|c_i'| = |c_i|$ for all $i$. The argument requires two steps:
> 1. **(McMillan)** $C$ uniquely decodable $\Rightarrow$ $\sum 2^{-\ell_i} \leq 1$.
> 2. **(Kraft converse)** $\sum 2^{-\ell_i} \leq 1$ $\Rightarrow$ there *exists* a prefix-free code with those lengths.
>
> Together, these imply that every uniquely decodable code can be replaced by a prefix-free code with identical lengths — and hence identical expected code length $\bar{\ell}$. We may therefore assume WLOG that any optimal code is prefix-free.

> [!question]- Proof (Kraft Converse — Existence of Prefix-Free Code)
> **Claim.** Given lengths $\ell_1 \leq \ell_2 \leq \cdots \leq \ell_n$ with $\sum_{i=1}^n 2^{-\ell_i} \leq 1$, there exists a prefix-free code $C'$ realising these lengths.
>
> **Construction (greedy).** Build the code iteratively on the full binary tree of depth $\ell_{\max}$:
> - For $i = 1, \ldots, n$ (in order of non-decreasing length):
>   - Choose the **leftmost available** node at depth $\ell_i$ as the codeword $c_i'$.
>   - Mark all descendants of $c_i'$ as *unavailable* (they can no longer be used as codewords or prefix-tree nodes).
>
> **Why does a free node always exist?** After assigning $c_1', \ldots, c_{i-1}'$, the number of nodes at depth $\ell_i$ that have been blocked is $\sum_{j < i} 2^{\ell_i - \ell_j}$. The total number of nodes at depth $\ell_i$ is $2^{\ell_i}$. A free node exists as long as:
>
> $$\sum_{j < i} 2^{\ell_i - \ell_j} < 2^{\ell_i} \iff \sum_{j < i} 2^{-\ell_j} < 1$$
>
> Since $\sum_{j=1}^n 2^{-\ell_j} \leq 1$, this holds strictly for every $i \leq n$. The construction therefore always succeeds, and the resulting code is prefix-free by design. $\square$

---

## 4. Deriving the Minimum Expected Code Length

We now solve the optimization problem. Since optimal codes are prefix-free and satisfy Kraft with equality, we have:

$$\min_{\ell_1, \ldots, \ell_n} \sum_{i=1}^{n} p_i \ell_i \qquad \text{subject to} \quad \sum_{i=1}^{n} 2^{-\ell_i} = 1, \quad \ell_i \geq 0$$

For a **continuous relaxation**, we temporarily drop the integer constraint on $\ell_i$.

**Variable substitution.** Let $q_i = 2^{-\ell_i}$, so $\ell_i = \log_2 \frac{1}{q_i}$ and the constraint becomes $\sum q_i = 1$. The problem transforms to:

$$\min_{\substack{q_1,\ldots,q_n \geq 0 \\ \sum q_i = 1}} \sum_{i=1}^{n} p_i \log_2 \frac{1}{q_i}$$

**Claim:** the minimum is achieved at $q_i^* = p_i$, giving $\bar{\ell}^* = \sum p_i \log_2 \frac{1}{p_i}$.

**Proof of optimality.** For any feasible $\{q_i\}$ with $\sum q_i = 1$:

$$\sum_{i=1}^{n} p_i \log_2 \frac{1}{q_i} - \sum_{i=1}^{n} p_i \log_2 \frac{1}{p_i} = \sum_{i=1}^{n} p_i \log_2 \frac{p_i}{q_i} \geq 0$$

The non-negativity follows from the **log inequality** $\log x \leq x - 1$ (equivalently $\ln x \leq x-1$):

$$\sum_{i=1}^{n} p_i \log_2 \frac{q_i}{p_i} \leq \frac{1}{\ln 2}\sum_{i=1}^{n} p_i \left(\frac{q_i}{p_i} - 1\right) = \frac{1}{\ln 2}\left(\sum_{i=1}^n q_i - \sum_{i=1}^n p_i\right) = 0$$

with equality if and only if $q_i = p_i$ for all $i$. $\square$

Therefore, the minimum expected code length in the continuous relaxation is:

$$\bar{\ell}^* = \sum_{i=1}^{n} p_i \log_2 \frac{1}{p_i} \quad \text{(bits)}$$

achieved by assigning code length $\ell_i^* = \log_2 \frac{1}{p_i}$ to symbol $x_i$.

---

## 5. Entropy

### 5.1 Definition

> [!note] Definition (Shannon Entropy)
> Let $X$ be a discrete random variable with probability distribution $P = (p_1, \ldots, p_n)$. The **entropy** of $X$ is:
>
> $$H(X) := \sum_{i=1}^{n} p_i \log_2 \frac{1}{p_i} \quad \text{(bits)}$$
>
> with the convention that $0 \log \frac{1}{0} = 0$.

The derivation above establishes directly that **entropy equals the minimum expected code length** under the continuous relaxation.

### 5.2 The Integer Constraint

In practice, code lengths must be positive integers. Using $\ell_i = \left\lceil \log_2 \frac{1}{p_i} \right\rceil$ (the *Shannon code*), one can show:

$$H(X) \leq \bar{\ell} < H(X) + 1 \quad \text{(bits)}$$

The lower bound holds because $\lceil x \rceil \geq x$. The upper bound follows from $\lceil x \rceil < x + 1$.

> [!important] Key Result
> Entropy is a fundamental lower bound on the expected code length of *any* uniquely decodable code. It can be approached to within 1 bit by the Shannon code, and this gap vanishes asymptotically when encoding long blocks.

### 5.3 Four Interpretations of Entropy

Entropy admits multiple equivalent readings, each illuminating a different facet:

| Interpretation | Description |
|---|---|
| **Minimum code length** | $H(X)$ bits per symbol are necessary and sufficient for lossless compression |
| **Information content** | The quantity $\log_2 \frac{1}{p_i}$ measures the "surprise" of observing $x_i$; entropy is its expectation |
| **Uncertainty** | $H(X)$ quantifies how uncertain we are about the outcome of $X$ before observing it |
| **Message complexity** | Entropy characterizes the irreducible complexity of the source |

> [!tip] Insight: Why Does $-\log p$ Measure Information?
> Rare events are more informative than common ones. If $p_i$ is very small, observing $x_i$ is surprising and carries substantial information; if $p_i \approx 1$, observing $x_i$ tells us almost nothing new. The function $\log_2 \frac{1}{p_i}$ is the unique measure (up to scaling) satisfying:
> 1. **Monotone:** more surprising events carry more information.
> 2. **Additive for independent events:** $I(p \cdot q) = I(p) + I(q)$.
> 3. **Continuous** in $p$.
>
> These three axioms uniquely determine $I(p) = \log_2 \frac{1}{p}$.

---

## Summary

```
Source Distribution P
        │
        ▼
Uniquely Decodable Code  ──(McMillan)──▶  Prefix-Free Code
        │                                        │
        │                              Kraft: Σ 2^{-ℓᵢ} = 1
        │                                        │
        ▼                                        ▼
  min Σ pᵢ ℓᵢ  ◀──────────────────  Optimization (qᵢ = pᵢ)
        │
        ▼
   H(X) = Σ pᵢ log₂(1/pᵢ)   ← Entropy
```

The chain of reasoning is tight: McMillan reduces the problem to prefix codes, the Kraft constraint translates lengths into a probability simplex, and the log inequality pins the optimum at the source distribution itself — yielding entropy as the inevitable answer.
