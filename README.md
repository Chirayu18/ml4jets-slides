# flashjet — ML4Jets 2026

Talk slides for **ML4Jets 2026** (Vienna, 14–18 September 2026).

**flashjet** is a GPU-native implementation of the generalised-$k_t$ jet
clustering algorithms (anti-$k_t$, $k_t$, Cambridge–Aachen). It clusters batches
of jets or events directly in GPU memory and returns both the particle-to-jet
assignment and the **complete merge history**, so substructure observables —
exclusive subjets, soft-drop grooming, Lund-plane coordinates — follow as cheap
array reads rather than a second clustering pass.

> flashjet is a **prototype**: the physics is validated and the speedups are
> real, but it is not integrated into any experiment's reconstruction framework.

## Building

```bash
latexmk -pdf flashjet-ml4jets.tex
```

Requires `pdflatex` and `beamer`. No external dependencies beyond a standard
TeX Live installation.

## Contents

| file | |
|---|---|
| `flashjet-ml4jets.tex` | slide source (beamer, 16:9) |
| `flashjet-ml4jets.pdf` | built deck |
| `make_illustrations.py` | regenerates the two schematic figures |
| `fig/` | figures |

The talk walks through how sequential recombination works, what the exponent $p$
selects (anti-$k_t$ / C--A / $k_t$), and then each substructure feature in turn:
exclusive subjets from the $k_t$ history, soft-drop grooming and Lund
coordinates from the C--A history.

## Data

All benchmarks use **ATLAS Open Data** (CC0), read over the public
`eospublic` redirector — no grid certificate or approval required:

- [ATLAS Top Tagging Open Data](https://opendata.cern.ch/record/15013)
  (ATL-PHYS-PUB-2022-039) — 2.48 M large-$R$ jets with calorimeter-cluster
  constituents. A [version with systematic uncertainties](https://opendata.cern.ch/record/80030)
  supersedes it.
- [2020 ATLAS Jet Reconstruction dataset](https://opendata.cern.ch/record/15010)
  — per-event clusters plus ATLAS's own reconstructed jets.

Physics closures use toy showers compared against analytic (leading-log)
predictions. Profiling figures are NVIDIA Nsight output — hardware counters, no
physics data.

## Headline results

- **Correctness:** 150/150 grid points (3 algorithms × 5 radii × 5 multiplicity
  bins × 2 samples, 20 000 jets/bin) at **100 % agreement** on the number of jets
  found, versus FastJet.
- **Closure against the experiment:** reproduces ATLAS's own `RecoJets_R4`
  exactly — 100 % jet-count match at ~600 clusters/event.
- **Speed:** **39–99×** over vectorised FastJet on an NVIDIA V100-class GPU,
  with the advantage growing with jet multiplicity. Cost is independent of the
  jet radius and of the algorithm choice.

The CPU baseline throughout is FastJet's **Python** binding; a C++ FastJet
comparison is the main outstanding measurement.

## License

Slides © Chirayu Gupta. ATLAS Open Data is released under CC0.
