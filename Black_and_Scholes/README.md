# A Two-Asset Extension of the Black–Scholes Derivation

## Motivation

After reading the original Black–Scholes paper, I wanted to gain a deeper understanding of the mechanics of option pricing by attempting a similar derivation in a multi-asset setting. Rather than modeling a single stock, I consider two underlying assets and derive a pricing equation under assumptions analogous to those used in the classical Black–Scholes framework.

This work should be viewed as an exploratory mathematical exercise intended to improve understanding of stochastic calculus and option pricing theory rather than a complete or validated financial model.

---

## Derivation Roadmap

```mermaid
flowchart TD

A[Two Stocks S1,S2<br>Prices Y1,Y2]
--> B[Define Weighted Portfolio<br>P = K1Y1 + K2Y2]

B --> C[Assume Mean Return<br>E[dP/P] = μ dt]

B --> D[Assume Variance<br>Var(dP/P)=σ²dt]

C --> E[Derive Drift Terms<br>E[dY₁]=μY₁dt<br>E[dY₂]=μY₂dt]

D --> F[Derive Covariance Structure<br>Var(dY)=YYᵀσ²dt]

F --> G[Obtain<br>Var(dY₁), Var(dY₂), Cov(dY₁,dY₂)]

E --> H[Construct Asset SDEs]

G --> H

H --> I[Apply Multivariate Itô Lemma]

I --> J[Option Dynamics<br>w(Y₁,Y₂,t)]

J --> K[Symmetry Assumption<br>w(Y₁,Y₂,t)<br>w(Y₂,Y₁,t)]

K --> L[Construct Hedged Portfolio]

L --> M[Eliminate Stochastic Terms]

M --> N[Nonlinear Two-Asset PDE]
```

---

# Model Setup

Let

$$
Y_1(t), \qquad Y_2(t)
$$

denote the prices of two stocks

$$
S_1,\qquad S_2.
$$

Consider a weighted portfolio

$$
P = K_1Y_1 + K_2Y_2,
$$

where $K_1$ and $K_2$ are constants.

The goal is to derive a pricing equation for options written on these assets by imposing assumptions on the portfolio return analogous to those used in the Black–Scholes framework.

---

# Assumptions

## Mean Return Assumption

Assume the expected percentage return of the weighted portfolio satisfies

$$E\!\left[\frac{dP}{P}\right]=
\mu\,dt.
$$

Writing this in vector form,

$$E\!\left[\frac{K^{T}dY}{K^{T}Y}\right]=
\mu\,dt,
$$

where

$$
K=
\begin{bmatrix}
K_1\\
K_2
\end{bmatrix},
\qquad
Y=
\begin{bmatrix}
Y_1\\
Y_2
\end{bmatrix}.
$$

Hence
$$K^{T}E(dY)=
K^{T}Y\,\mu\,dt.
$$

Since this must hold for arbitrary $K$,

$$
E(dY_1)=\mu Y_1dt,
$$

$$
E(dY_2)=\mu Y_2dt.
$$

Therefore the drift terms satisfy

$$
a_1=\mu Y_1,
$$

$$
a_2=\mu Y_2.
$$

---

## Variance Assumption

Assume

$$\operatorname{Var}\left(\frac{K^{T}dY}{K^{T}Y}\right)=
\sigma^2 dt.
$$

Using
$$\operatorname{Var}(AX)=
A\,\operatorname{Var}(X)\,A^T,
$$

gives

$$\frac{K^T\operatorname{Var}(dY)K}{(K^TY)^2}=
\sigma^2dt.
$$

Thus

$$K^T\operatorname{Var}(dY)K=
K^TY\,Y^TK\,\sigma^2dt.
$$

Since this relation must hold for arbitrary $K$,

$$\boxed{
\operatorname{Var}(dY)=
YY^T\sigma^2dt
}
$$

or

$$
\operatorname{Var}(dY)=
\sigma^2dt
\begin{bmatrix}
Y_1^2 & Y_1Y_2\\
Y_1Y_2 & Y_2^2
\end{bmatrix}.
$$

Therefore

$$
\boxed{
\operatorname{Var}(dY_1)=
\sigma^2Y_1^2dt
}
$$

$$
\boxed{
\operatorname{Var}(dY_2)=
\sigma^2Y_2^2dt
}
$$

and

$$
\boxed{
\operatorname{Cov}(dY_1,dY_2)=
\sigma^2Y_1Y_2dt
}.
$$

---

# Asset Dynamics

Assume the stochastic differential equations

$$
dY_1=
a_1(Y_1)\,dt
+
b_1(Y_1)\,dB_1
+
b_2(Y_1)\,dB_2
$$

and

$$
dY_2=
a_2(Y_2)\,dt
+
c_1(Y_2)\,dB_1
+
c_2(Y_2)\,dB_2.
$$

Using the covariance structure obtained above,

$$
Y_1^2\sigma^2=
b_1^2+b_2^2
\tag{1}
$$

$$
Y_2^2\sigma^2=
c_1^2+c_2^2
\tag{2}
$$

$$
Y_1Y_2\sigma^2=
b_1c_1+b_2c_2.
\tag{3}
$$

Together with

$$
a_1=\mu Y_1,
\qquad
a_2=\mu Y_2,
$$

these equations characterize the admissible diffusion coefficients.

---

# Option Price Dynamics

Let

$$
w(Y_1,Y_2,t)
$$

denote the option price associated with stock $S_1$.

Applying multivariate Itô's lemma,

$$
dw=
\frac{\partial w}{\partial t}dt
+
\frac{\partial w}{\partial y_1}dY_1
+
\frac{\partial w}{\partial y_2}dY_2
$$

$$
+
\frac12
\sigma^2Y_1^2
\frac{\partial^2 w}{\partial y_1^2}
dt
+
\frac12
\sigma^2Y_2^2
\frac{\partial^2 w}{\partial y_2^2}
dt
$$

$$
+
\sigma^2Y_1Y_2
\frac{\partial^2 w}
{\partial y_1\partial y_2}
dt.
$$

Equivalently,

$$
\Delta w=
\frac{\partial w}{\partial t}\Delta t
+
\frac{\partial w}{\partial y_1}\Delta y_1
+
\frac{\partial w}{\partial y_2}\Delta y_2
$$

$$
+
\frac12
\frac{\partial^2 w}{\partial y_1^2}
\sigma^2Y_1^2\Delta t
+
\frac12
\frac{\partial^2 w}{\partial y_2^2}
\sigma^2Y_2^2\Delta t
$$

$$
+
\frac{\partial^2 w}
{\partial y_1\partial y_2}
\sigma^2Y_1Y_2\Delta t.
$$

---

# Symmetry Assumption

Assume a symmetric pricing function

$$
w(Y_1,Y_2,t)=
\text{Option price of }S_1
$$

and

$$
w(Y_2,Y_1,t)=
\text{Option price of }S_2.
$$

This assumption allows both option prices to be represented by the same function with interchanged arguments.

---

# Hedging Argument

Construct a portfolio consisting of weighted option positions

$$
\Pi=
K_1w(Y_1,Y_2,t)
+
K_2w(Y_2,Y_1,t).
$$

Following a Black–Scholes style hedging argument and imposing a return-neutrality condition leads to the nonlinear PDE

$$
\gamma
\left[
\left(\frac{\partial w}{\partial y_1}\right)^2-
\left(\frac{\partial w}{\partial y_2}\right)^2
\right]
(K_1y_1+K_2y_2)
$$

$$
-\gamma
\frac{\partial w}{\partial y_1}
\Big[
K_1w(y_1,y_2,t)
+
K_2w(y_1,y_2,t)
\Big]
$$

$$
+\gamma
\frac{\partial w}{\partial y_2}
\Big[
K_2w(y_1,y_2,t)
+
K_1w(y_2,y_1,t)
\Big]
$$

$$
+\frac{\sigma^2}{2}\left(K_1y_1^2+K_2y_2^2\right)\left(\frac{\partial^2 w}{\partial y_1^2}\frac{\partial w}{\partial y_1}-
\frac{\partial^2 w}{\partial y_2^2}
\frac{\partial w}{\partial y_2}
\right)
$$

$$
-\frac{\sigma^2}{2}
\left(
K_1y_2^2+K_2y_1^2
\right)
\left(
\frac{\partial^2 w}{\partial y_1^2}
\frac{\partial w}{\partial y_2}-
\frac{\partial^2 w}{\partial y_2^2}
\frac{\partial w}{\partial y_1}
\right)
=0.
$$

---

# Mathematical Structure

The derivation can be summarized schematically as

$$
\boxed{
\text{Portfolio Assumptions}
\Longrightarrow
\text{Covariance Matrix}
\Longrightarrow
\text{Asset SDEs}
\Longrightarrow
\text{It\^o Expansion}
\Longrightarrow
\text{Hedging}
\Longrightarrow
\text{Pricing PDE}
}
$$

---

# Conclusion

This project began as an attempt to better understand the ideas underlying the Black–Scholes derivation by extending the argument to a simple two-asset setting. Starting from assumptions on the mean and variance of returns of a weighted portfolio, I derived the implied covariance structure, constructed stochastic differential equations for the underlying assets, and applied multivariate Itô calculus together with a hedging argument to obtain a nonlinear pricing PDE.

While I still have a great deal to learn in stochastic calculus—particularly the deeper mathematical foundations and derivation of Itô's lemma—this exercise was extremely valuable. It provided practical experience in applying various forms of the chain rule, including multivariate extensions, working with covariance matrices, constructing stochastic differential equations, and understanding how hedging arguments transform stochastic dynamics into deterministic partial differential equations.

More importantly, the exercise highlighted how much of option pricing theory is built from a careful combination of probability, differential calculus, and financial reasoning. Even if the resulting PDE does not correspond to a practical pricing model, the process of deriving it proved to be an excellent learning experience and provided a deeper appreciation for the structure of the Black–Scholes framework.

---

# Future Directions

- Study rigorous derivations of Itô's lemma and stochastic integrals.
- Extend the model to correlated Brownian motions.
- Investigate arbitrage-freeness of the resulting framework.
- Analyze the nonlinear PDE and its mathematical properties.
- Explore appropriate terminal and boundary conditions.
- Develop finite-difference and Monte Carlo solution methods.
- Compare the derivation with established multi-asset Black–Scholes models.
- Investigate the existence of an equivalent risk-neutral measure.

---

# Disclaimer

This work was carried out as a personal learning exercise inspired by the original Black–Scholes paper. The derivation is exploratory and should not be interpreted as a validated financial pricing model. Its primary purpose is to deepen understanding of stochastic calculus, option pricing, and multidimensional extensions of classical financial models.
