# Disruption Thresholds in a 2‑D Ising Model (Retro B/W)

A compact, dependency‑free exploration of how **small fixed minorities** can stall or prevent consensus in a locally aligning system. The model is a 2‑D ferromagnetic Ising lattice with a fraction `p` of permanently negative spins (“disruptors”) that never update.

---

## What this is

* **Interactive simulation** of a square Ising lattice using a minimalist black‑and‑white aesthetic. Each cell is a DOM node in a CSS grid (no `<canvas>`), so the state is visually inspectable.
* **Qualitative study** of disruption thresholds: how tiny fractions of fixed `−1` sites can cause:

  * **Big slow‑downs** in reaching high magnetization (consensus), and
  * **Breakdown** where strong consensus isn’t reached within reasonable time budgets.
* **Built‑in experiment runner** to sweep lattice sizes `L` and disruptor fractions `p`, reporting success rates and time‑to‑converge statistics.

---

## Model, in brief

* **Spins:** `sᵢ ∈ {−1, +1}` on an `L × L` periodic lattice (toroidal wrap).
* **Pinned disruptors:** with probability `p`, a site is marked fixed and clamped to `−1` for the entire run.
* **Energy:** `E = −J ∑⟨ij⟩ sᵢ sⱼ − h ∑ᵢ sᵢ` with `J = 1` (local alignment) and uniform field `h ≥ 0` (top‑down push toward `+1`).
* **Dynamics (Metropolis):** pick a random unfixed site; flip with probability `min(1, exp(−ΔE/T))`, where `ΔE = 2 sᵢ (J ∑_nn s + h)` and `T` is temperature.
* **Magnetization:** `m = (1/N) ∑ sᵢ`. A run is considered “converged” if `m ≥ m*` for `K` consecutive frames.

---

## What you can explore

### Live Demo

* **Lattice size `L`** — changes system scale and sharpness of transitions.
* **Temperature `T`** — higher `T` injects noise, competing with alignment.
* **Field `h`** — a gentle global bias toward `+1`.
* **Pinned fraction `p`** — density of fixed `−1` disruptors.
* **Sweeps per frame** — compute budget per animation frame.
* **Convergence target `m*` & frames `K`** — define what counts as “consensus.”

**Actions**: Initialize/reset, re‑sample the pinned mask, start/stop, step, and randomize spins. The status line shows `m`, number of pinned sites, sweep count, and run state. White cells are `+1`, black cells are `−1`, dashed outlines mark pinned disruptors.

### Experiment Runner

Run small grid searches over `L` and `p` to quantify:

* **Success rate** — fraction of trials that achieve `m ≥ m*` for `K` frames within a sweep budget.
* **Avg/Median sweeps (success)** — time‑to‑threshold among successful runs.

From these, the UI reports two heuristic thresholds per `L`:

* **Big slow‑down `p*`** — first `p` where average time ≥ `2×` the baseline at `p=0`.
* **Breakdown `p†`** — first `p` where the success rate drops to `0`.

---

## Typical qualitative pattern

* **Small `p` (\~1–3%)** — convergence becomes much slower (often ≈2× baseline).
* **Moderate `p` (\~5–10%)** — strong consensus frequently fails within practical budgets.
* **Larger `L`** — transitions sharpen and the critical `p` shifts lower.

Exact values depend on `T`, `h`, `L`, the convergence definition (`m*`, `K`), and the sweep budget.

---

## Design choices

* **Periodic boundary conditions** are implemented with modular indexing; neighbor sums are computed per update.

---

## Notes & limitations

* **Heuristic thresholds.** `p*` and `p†` are operational, not analytical, and depend on your chosen budgets.
* **Stochastic runs.** Results vary between runs; there is no seeded RNG by default.
* **Scope.** The model is 2‑D, nearest‑neighbor, homogeneous field; it’s meant for intuition‑building, not precise inference.

---

## Why it’s interesting

This toy captures a common systems intuition: **small, stubborn minorities can dramatically alter collective dynamics** even when most agents align locally and a weak global signal exists. The sharpness of the transition with scale echoes real‑world coordination problems where size and structure matter.
