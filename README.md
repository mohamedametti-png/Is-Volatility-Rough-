# Is Volatility Rough?
### Statistical Roughness vs Intrinsic Roughness in Volterra Stochastic Volatility Models

---

## Overview

Since the seminal work of Gatheral, Jaisson and Rosenbaum (2018), financial volatility has been widely described as *rough*, with empirical estimates of the Hurst exponent around $H \approx 0.1$.  

However, recent research (Abi Jaber & Li, 2024) questions whether observed statistical roughness truly reflects intrinsic roughness of the underlying spot volatility process.

This repository investigates the question:

> **Does statistical roughness necessarily imply intrinsic roughness of volatility?**

We conduct a numerical experiment within the Volterra stochastic volatility framework, comparing asymptotic roughness implied by short-maturity skew calibration with empirical Hurst estimates obtained from simulated trajectories.

---

## Model Framework

We consider a Volterra–Heston type variance process:

\[
V_t = g_0(t)
+ \int_0^t K(t-s)\, b V_s ds
+ \int_0^t K(t-s)\, c \sqrt{V_s} dW_s.
\]

Three kernels are studied:

- **Exponential kernel** (Markovian / classical Heston)
- **Fractional power-law kernel** (rough Heston)
- **Shifted power-law kernel** (regularized hyper-rough)

All models are calibrated to the short-maturity ATM skew.

---

## Short-Maturity Skew and Implicit Hurst Exponent

For power-law kernels, short-maturity asymptotics predict:

\[
\text{Skew}_{ATM}(T) \sim C T^{H-\frac12}.
\]

From calibration:

\[
H_{skew} = \beta + \frac12.
\]

This provides an *implicit structural estimate* of the Hurst exponent.

---

## Simulation Methodology

We simulate the integrated variance process:

\[
U_t = \int_0^t V_s ds,
\]

using the **iVi (integrated Volterra implicit) scheme**  
(Abi Jaber & Attal, 2025), which:

- Preserves positivity
- Handles singular kernels
- Remains stable for rough dynamics
- Avoids explicit reconstruction of $V$

Spot variance is approximated by:

\[
V_{t_i} \approx \frac{U_{i-1,i}}{\Delta t}.
\]

We primarily work with **log-volatility**:

\[
X_t = \log(\sigma_t), \quad \sigma_t = \sqrt{V_t}.
\]

---

## Roughness Estimation

Following Gatheral, Jaisson and Rosenbaum (2018), we compute empirical $q$-variations:

\[
m(q,\Delta)
=
\frac{1}{N-\Delta}
\sum_{i=0}^{N-\Delta-1}
|X_{i+\Delta} - X_i|^q.
\]

Under monofractal scaling:

\[
m(q,\Delta) \sim C_q \Delta^{qH}.
\]

The Hurst exponent is estimated via log–log regression.

---

## Numerical Results

| Model        | $H_{skew}$ | Estimated $\widehat H$ |
|-------------|------------|-------------------------|
| Exponential | 0.5        | ≈ 0.48                  |
| Fractional  | 0.24       | ≈ 0.14                  |
| Shifted     | -0.20      | ≈ 0.38                  |

### Key Observations

- A Markovian model can produce $\widehat H < 0.5$ on finite samples.
- A hyper-rough asymptotic kernel may not appear strongly rough at finite resolution.
- Statistical roughness is highly scale-dependent.
- Mean reversion distorts large-scale behavior.
- Discretization and volatility proxies influence estimation.

---

## Implications for Option Pricing

Rough volatility models imply:

- Specific short-maturity smile asymptotics
- Strong small-scale memory effects
- Distinct forward variance dynamics

Misinterpreting statistical roughness as intrinsic roughness may:

- Lead to overfitting short-dated implied volatility
- Overstate volatility-of-volatility
- Misprice short-maturity options
- Distort hedging sensitivities

Statistical Hurst estimation alone should therefore **not** serve as a decisive model selection criterion.

---

## Conclusion

This numerical experiment supports the findings of Abi Jaber & Li (2024):

> Observed statistical roughness does not necessarily imply intrinsic roughness of spot volatility.

Roughness may emerge as an *effective finite-scale property* rather than a structural feature of the underlying model.

---

## References

- Gatheral, Jaisson, Rosenbaum (2018)
- Abi Jaber & Li (2024)
- Abi Jaber & Attal (2025)

---

## Author

Mohamed Ametti  
Master 2 Probabilités et Finance  
Sorbonne Université
