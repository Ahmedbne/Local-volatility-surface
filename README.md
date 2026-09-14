# Arbitrage-Free Local-Volatility Surface from Option Quotes

**Ahmed Bounadar — September 2026**

## What this is

An end-to-end quant project: build a local-volatility surface from noisy option
quotes and use it to price exotics.

Pipeline: synthetic Heston market (with realistic quote noise) → implied-vol
inversion → arbitrage-free eSSVI calibration → Dupire's equation → local-vol
Monte Carlo → exotic pricing vs. Heston and flat Black–Scholes.

The core point of the project: differentiating noisy option prices directly
(as Dupire's formula naively requires) is an ill-posed problem — ~26% of raw
estimates come out negative. Fitting an arbitrage-free parametric surface
first fixes this *and* denoises the data (fit is 2x closer to the truth than
the quotes it was fitted to).

## Files

| File | Contents |
|---|---|
| `report.pdf` | Full write-up: proofs, math, results, references (26 pages) |
| `local_volatility_surface.ipynb` | Fully executed notebook, code + all figures |

## Requirements to re-run

Python 3, with `numpy`, `scipy`, `pandas`, `matplotlib`. No other dependencies —
Black–Scholes, Heston, SVI/SSVI, Dupire, and the Monte Carlo engines are all
implemented from scratch in the notebook. Runtime: ~4 minutes end to end.

## Key results

- Fitted surface: 41.8 bp error vs. noisy quotes, but only 21.3 bp vs. the
  (normally unobservable) true surface — regularization denoises.
- Local vol recovered to ~1 vol point inside the quoted strike range;
  meaningless outside it (no data ⇒ no information).
- Local-vol Monte Carlo reprices the input vanillas to 13 bp (confirms
  Dupire's theorem numerically).
- Local vol vs. true Heston on a tight up-and-out barrier: **off by up to 56%**
  — same vanillas, very different exotic price. That gap is pure model risk.
