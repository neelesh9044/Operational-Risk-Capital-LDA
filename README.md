# Operational Risk Regulatory Capital — Loss Distribution Approach

A Python implementation of the Loss Distribution Approach (LDA) used under the
Basel Advanced Measurement Approach (AMA) to derive operational risk regulatory
capital from modelled loss frequency and severity.

## Method

Operational losses over a one-year horizon are modelled as a compound distribution:

| Component | Distribution | Parameters |
|---|---|---|
| Loss frequency | Poisson | λ = 5 events per year |
| Loss severity | Lognormal | mean €1m, variance 2.25m EUR² |

The aggregate annual loss distribution is obtained by **Monte Carlo simulation**:
for each scenario, draw an event count from the Poisson distribution, then draw
that many independent lognormal severities and sum them. Repeating this builds the
empirical aggregate loss distribution, from which risk measures are read off as
quantiles.

Capital is reported as **Value at Risk and Expected Shortfall at the 90%, 95%, 99%
and 99.5% levels.**

## Scope and limitations

Stated plainly, because they matter if you're reading this as evidence of
production capability:

- **Parameters are assumed, not fitted.** Frequency and severity parameters are
  specified rather than estimated from an internal loss dataset, so there are no
  goodness-of-fit diagnostics.
- **Confidence levels stop at 99.5%.** The Basel AMA soundness standard is 99.9%
  over a one-year horizon; the levels here are illustrative of the method rather
  than regulatory-calibrated.
- **Single loss category.** A production model would run the LDA per business
  line and event type, then aggregate across cells with a dependence structure
  rather than treating losses as one homogeneous pool.

## A note on AMA and Basel III

The Advanced Measurement Approach was withdrawn under the Basel III reforms and
replaced by the Standardised Measurement Approach, with implementation from 2023.
This project implements AMA as the methodology taught. The underlying mechanics —
separating frequency from severity, compounding them into an aggregate loss
distribution, and reading capital off a high quantile — remain the analytical
foundation of operational risk measurement, and continue to be used internally for
economic capital and stress testing even where the regulatory approach has changed.

## Files

| File | What it is |
|---|---|
| `operational_risk_capital_LDA.ipynb` | Full notebook: distributions, Monte Carlo aggregation, VaR and Expected Shortfall |

## Stack

Python, NumPy, SciPy (`stats`), Matplotlib.

## Context

Individual project completed during the MSc in Finance (Quantitative and Financial
Markets track) at emlyon business school.
