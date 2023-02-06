---
marp: true
header: 虚時間グリーン関数に対するスパースモデリング入門
footer: ©2023 品岡寛
# Page number
paginate: true
---

<!-- To show total number of pages for paginate = true -->
<style>
section::after {
  content: attr(data-marpit-pagination) '/' attr(data-marpit-pagination-total);
}
</style>

虚時間グリーン関数に対するスパースモデリング入門
===

##### 品岡寛 (埼玉大学)

---
# 前提知識

* 虚時間形式グリーン関数の基礎
* Python or Juliaの基礎知識
   - 基本文法
   - 多次元配列
   - ...

---
# 何を学べるの？

* 虚時間グリーン関数のコンパクトな中間表現基底
  - $G(\tau) = \sum_{l=0}^{L-1} U_l(\tau) g_l$
  - $L\propto \log \beta W$ ($\beta$: inverse temperature, $W$: band width)
  - $L \propto \log (1/\epsilon)$ ($\epsilon$: desired accuracy)
* 虚時間・虚周波数におけるスパースメッシュ: # of points $\simeq L$.
* SparseIR.jl (Julia), sparse-ir (Python)

---
# 参考資料

* 固体物理の記事 (古いライブラリirbasisに基づいてます)
* $\uparrow$の英語訳・加筆 + 新ライブラリsparse-irに更新 

---
# 概要

1. 虚時間グリーン関数の性質のまとめ
2. 中間表現基底
3. スパースサンプリング法
4. 関連技術紹介



---
# Imaginary-time Green's functions

Also known as Matsubara Green's functions:

$$
G(\tau) = - \langle T_\tau A(\tau) B(0) \rangle,
$$

where
* $A(\tau)$, $B(\tau)$ are operators in the Heisenberg picture ($A(\tau) = e^{\tau H} A e^{-\tau H}$).
* $\langle \cdots \rangle = \mathrm{Tr}(e^{-\beta H} \cdots)$, where $\beta = 1/T$ ($k_\mathrm{B}=1$).

We use the Hamiltonian formalism throughout this lecture.

---
# Imaginary-time Green's functions

$$
G(\tau) = - \langle T_\tau A(\tau) B(0) \rangle
$$

* $A$ and $B$ are fermionic operators $\rightarrow$ $G(\tau) = -G(\tau+\beta)$
* $A$ and $B$ are bosonic operators $\rightarrow$ $G(\tau) = G(\tau+\beta)$

In general, $G(\tau)$ has a discontinuity at $\tau = n \beta$ ($n \in \mathbb{N}$).

---
# Imaginary-frequency (Matsubara) Green's functions

Matsubara Green's function:

$$
G(\mathrm{i}\omega) = \int_0^\beta \mathrm{d}\tau e^{\mathrm{i} \omega \tau} G(\tau).
$$

From $G(\tau+\beta) = \mp G(\tau)$,

*  $\omega = (2n+1) T\pi$ (fermion)
*  $\omega = 2n T \pi$ (boson)
  
($n \in \mathbb{N}$)

These discrete imaginary frequencies are denoted as Matsubara frequencies.

---
# Spectral/Lehmann representation

$$
G(z) = \int_{-\infty}^{+\infty} \mathrm{d}\omega' \frac{\rho(\omega')}{z - \omega'},
$$
where $\rho(\omega)$ is a spectral function.

* $z = \mathrm{i}\omega$ $\rightarrow$ Matsubara Green's function
* $z = \omega + \mathrm{i}0^+$ $\rightarrow$ Retarded Green's function (not used in this lecture)


---
# How Greeen's function look like in $\tau$?

Example (single pole): $\rho(\omega) = \delta(\omega - \omega_0)$, $\omega_0 > 0$

$$
G(\mathrm{i}\omega) = \frac{1}{\mathrm{i}\omega - \omega_0}
$$

$$
G(\tau) = - \frac{e^{-\tau\omega_0}}{1 + e^{-\beta\omega_0}}~(0 < \tau < \beta)
$$


At $\tau\approx 0$, $G(\tau) \propto e^{-\tau\omega_0}$.
For $\beta \omega_0 \gg 1$, coexisting two time scales : $1/\omega_0 \ll \beta$ 


---
# How Greeen's function look like in Matsubara frequency space


Example (single pole): $\rho(\omega) = \delta(\omega - \omega_0)$, $\omega_0 > 0$

$$
G(\mathrm{i}\omega) = \frac{1}{\mathrm{i}\omega - \omega_0}
$$

$$
G(\tau) = - \frac{e^{-\tau\omega_0}}{1 + e^{-\beta\omega_0}}~(0 < \tau < \beta)
$$


At high frequencies $|\omega| \gg |\omega_0|$, $G(\mathrm{i}\omega) \approx 1/(\mathrm{i}\omega)$.

For $\beta \omega_0 \gg 1$, coexisting two energy scales: $\omega_0 \ll T = 1/\beta$ 

---
# Difficulties in numerical simulations

If band width $W$ and temperature $T$ differ by orders of magnitudes as $\beta W \gg 1$:

* Slow power-law decay at high frequencies $\rightarrow$ Large truncation errors
* Uniform dense mesh in $\tau$ requires a huge number of points $\propto \beta W$.

Example:

* Band width 10 eV, superconducting temperature 1 K $\approx 0.1$ meV $\rightarrow$ $\beta W = 10^5$.

We need a compact basis with exponetial convergence.

---
# Compact representations

* **Intermediate represenation** (sparse-ir)
  - *Ab initio* calculations (Eliashberg theory, *GW*, Lichtenstein formula)
  - Diagrammatic calculations (FLEX)
* Discrete Lehmann representation (implemented in sparse-ir as well)
* Minmax method (from Kresse's group)


---
# SVD of kernel

---
# Singular values 

---
# How basis functions look like?

---
# Expansion in IR basis

---
# Sparse meshes in frequency and time domains

---
# Numerical Fourier transform


---
# Implementation of Dyson equation

---
# Implementation of second-order perturbation theory


---
# Related technologies

* Minmax
* Discrete Lehmann reprensetation