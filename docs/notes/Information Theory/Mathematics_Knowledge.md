---
tags:
  - Mathematics
  - Reference
  - LinearAlgebra
  - Calculus
date: 2026-03-03
---

# Mathematics Knowledge Base

This note serves as a centralized repository for mathematical concepts encountered during the study of Information Theory and related fields.

## Linear Algebra Foundations

### Eigenvalues and Generalized Eigenvectors

In linear difference equations, we often need to calculate $M^t v_0$. To do this efficiently, we want to represent $v_0$ as a linear combination of vectors that are easy to multiply by $M$.

**Ideally (Diagonalizable Matrix):**
If $M$ has $k$ linearly independent eigenvectors $u_1, \dots, u_k$ (which happens if all eigenvalues are distinct), they form a basis for $\mathbb{R}^k$.
Any initial vector $v_0$ can be uniquely written as:
$$ v_0 = a_1 u_1 + \dots + a_k u_k $$
Then:
$$ M^t v_0 = a_1 \lambda_1^t u_1 + \dots + a_k \lambda_k^t u_k $$

**In General (Jordan Canonical Form):**
If $M$ has repeated eigenvalues and is **defective** (not enough eigenvectors), we cannot form a basis with just eigenvectors. We need **Generalized Eigenvectors**.

- A generalized eigenvector $w$ of rank $m$ for eigenvalue $\lambda$ satisfies $(M - \lambda I)^m w = 0$.
- These vectors, together with regular eigenvectors, *always* form a basis for $\mathbb{R}^k$.
- The action of $M$ on a generalized eigenvector involves terms like $t \lambda^t, t^2 \lambda^t$, etc. (similar to solutions like $t e^{\lambda t}$ in differential equations).

> [!important] Key Takeaway
> Regardless of whether $M$ is diagonalizable, the growth of $M^t$ is dominated by the eigenvalue with the **largest absolute value** (spectral radius).

## Linear Difference Equations

> [!note] Context: Shannon's 1948 Paper
> In Part I, Section 1, Shannon models the number of allowed signal sequences $N(t)$ using a linear difference equation.
> 
> $$N(t) = N(t-t_1) + N(t-t_2) + \dots + N(t-t_n)$$

### What is a Difference Equation?

A **Difference Equation** (or Recurrence Relation) is the discrete analog of a **Differential Equation**.
- **Differential Equation**: Relates a function $f(t)$ to its derivatives (rate of change) in continuous time.
- **Difference Equation**: Relates a term in a sequence $N(t)$ to previous terms $N(t-1), N(t-2), \dots$ in discrete steps.

### Solving Linear Homogeneous Difference Equations

For an equation of the form:
$$N(t) = c_1 N(t-1) + c_2 N(t-2) + \dots + c_k N(t-k)$$

We guess a solution of the form $N(t) = X^t$. Substituting this into the equation:

$$X^t = c_1 X^{t-1} + c_2 X^{t-2} + \dots + c_k X^{t-k}$$

Dividing by $X^{t-k}$ (assuming $X \neq 0$):

$$X^k - c_1 X^{k-1} - \dots - c_k = 0$$

This polynomial is called the **Characteristic Equation**.

> [!question]- Why guess $N(t) = X^t$?
> 
> **Intuition: Matrix Transition & Eigenvalues**
> 
> We can rewrite any $k$-th order linear difference equation as a first-order matrix equation.
> Let the state vector at time $t$ be $v_t = \begin{bmatrix} N(t) \\ N(t-1) \\ \vdots \\ N(t-k+1) \end{bmatrix}$.
> 
> Then the recurrence $N(t) = c_1 N(t-1) + \dots + c_k N(t-k)$ becomes:
> 
> $$ v_t = M v_{t-1} $$
> 
> where $M$ is the **Transition Matrix** (specifically a Companion Matrix):
> $$
> M = \begin{bmatrix} 
> c_1 & c_2 & \dots & c_k \\
> 1 & 0 & \dots & 0 \\
> 0 & 1 & \dots & 0 \\
> \vdots & \vdots & \ddots & \vdots \\
> 0 & 0 & \dots & 1 
> \end{bmatrix}
> $$
> 
> By iterating this, we get:
> $$ v_t = M^t v_0 $$
> 
> If we decompose $v_0$ into the eigenvectors $u_1, \dots, u_k$ of $M$ (with eigenvalues $\lambda_1, \dots, \lambda_k$):
> $$ v_0 = a_1 u_1 + \dots + a_k u_k $$
> 
> Then applying $M^t$:
> $$ v_t = a_1 \lambda_1^t u_1 + \dots + a_k \lambda_k^t u_k $$
> 
> The scalar $N(t)$ (the first component of $v_t$) will thus be a linear combination of $\lambda_i^t$.
> This is why we assume the solution is of the form $X^t$ — the base $X$ is simply an **eigenvalue** of the transition matrix.

### Asymptotic Behavior

If the characteristic equation has distinct roots $X_1, X_2, \dots, X_k$, the general solution is:

$$N(t) = a_1 X_1^t + a_2 X_2^t + \dots + a_k X_k^t$$

For large $t$, the term with the **largest absolute value** (let's call it $X_0$) will dominate. Thus:

$$N(t) \approx a_0 X_0^t$$

Taking the logarithm (as Shannon does for Capacity):

$$\lim_{t \to \infty} \frac{\log N(t)}{t} = \lim_{t \to \infty} \frac{\log(a_0 X_0^t)}{t} = \lim_{t \to \infty} \frac{\log a_0 + t \log X_0}{t} = \log X_0$$

This explains why Shannon says the capacity $C$ is $\log X_0$, where $X_0$ is the largest real root of the characteristic equation.
