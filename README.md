# TopoBench A123 Contribution Snapshot

> **Recruiter note:** This repository is a TopoBench snapshot documenting my A123 auditory cortex graph-construction and CWN tutorial contribution. See [`README_contribution.md`](./README_contribution.md) for the detailed explanation of my individual work.

## What I contributed

I implemented A123 auditory cortex graph construction for TopoBench by pooling neurons across all five cortical layers for each `(session, best-frequency bin)` pair. The implementation collects neuron indices from every cortical layer, deduplicates them, slices the shared signal/noise correlation matrices, and emits one graph-level PyTorch Geometric sample per frequency bin.

I also added tutorial coverage for training TopoBench/TBModel on the A123 auditory cortex dataset, including a CWN + TopoModelX graph-level classification walkthrough.

## Key files to review

- [`README_contribution.md`](./README_contribution.md) — detailed recruiter-facing summary of my contribution.
- [`topobench/data/datasets/a123.py`](./topobench/data/datasets/a123.py) — A123 graph construction and PyTorch Geometric data conversion logic.
- [`tutorials/tutorial_train_brain_model_CWN.ipynb`](./tutorials/tutorial_train_brain_model_CWN.ipynb) — cleaned CWN/TopoModelX tutorial for A123 graph-level classification.
- [`tutorials/tutorial_train_a123_clean.ipynb`](./tutorials/tutorial_train_a123_clean.ipynb) — clean general A123 training walkthrough.

## Related pull requests

- [PR #1](https://github.com/prathyush-sinha/TopoBench-pr252-5e0a0f5/pull/1) — original A123 graph-construction and tutorial contribution.
- [PR #2](https://github.com/prathyush-sinha/TopoBench-pr252-5e0a0f5/pull/2) — cleanup PR that added recruiter-facing documentation and clean tutorial material.

## Technical context

This work uses TopoBench and TopoModelX for topological deep learning workflows. The A123 task is graph-level classification of best-frequency bins from neural correlation graphs built from mouse auditory cortex calcium-imaging data.

The important engineering contribution is the dataset-processing path: session/frequency-bin graph construction, correlation-thresholded graph generation, PyG `Data` object creation, and tutorial integration with CWN-style topological neural network training.

## Results/status

This snapshot documents the data-processing and model-integration work. No final benchmark metrics are claimed in this README because validated training logs or a metrics table are not committed yet.

## Upstream project

TopoBench is a framework for benchmarking topological deep learning models. The original upstream project is maintained by the Geometric Intelligence / Topological Intelligence ecosystem. This repository is a contribution snapshot/fork-style working copy used to show my A123-related implementation work.
