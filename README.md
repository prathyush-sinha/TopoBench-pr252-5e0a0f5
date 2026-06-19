# TopoBench A123 Contribution Snapshot

> **Note:** This repository is a TopoBench snapshot documenting my A123 auditory cortex graph-construction and CWN tutorial contribution. See [`README_contribution.md`](./README_contribution.md) for the detailed explanation of my individual work.

## What I contributed

I implemented A123 auditory cortex graph construction for TopoBench by pooling neurons across all five cortical layers for each `(session, best-frequency bin)` pair. The implementation collects neuron indices from every cortical layer, deduplicates them, slices the shared signal/noise correlation matrices, and emits one graph-level PyTorch Geometric sample per frequency bin.

I also added tutorial coverage for training TopoBench/TBModel on the A123 auditory cortex dataset, including a CWN + TopoModelX graph-level classification walkthrough.

## Key files to review

- [`README_contribution.md`](./README_contribution.md) — detailed summary of my contribution.
- [`topobench/data/datasets/a123.py`](./topobench/data/datasets/a123.py) — A123 graph construction and PyTorch Geometric data conversion logic.
- [`tutorials/tutorial_train_brain_model_CWN.ipynb`](./tutorials/tutorial_train_brain_model_CWN.ipynb) — cleaned CWN/TopoModelX tutorial for A123 graph-level classification.
- [`tutorials/tutorial_train_a123_clean.ipynb`](./tutorials/tutorial_train_a123_clean.ipynb) — clean general A123 training walkthrough.

## Related pull requests

- [PR #1](https://github.com/prathyush-sinha/TopoBench-pr252-5e0a0f5/pull/1) — original A123 graph-construction and tutorial contribution.
- [PR #2](https://github.com/prathyush-sinha/TopoBench-pr252-5e0a0f5/pull/2) — cleanup PR that added documentation and clean tutorial material.

## Technical context

This work uses TopoBench and TopoModelX for topological deep learning workflows. The A123 task is graph-level classification of best-frequency bins from neural correlation graphs built from mouse auditory cortex calcium-imaging data.

The important engineering contribution is the dataset-processing path: session/frequency-bin graph construction, correlation-thresholded graph generation, PyG `Data` object creation, and tutorial integration with CWN-style topological neural network training.

## Experimental results

The project evaluated graph, simplicial-complex, cellular-complex, hypergraph, and point-cloud style models on the A123 graph-level classification task. Results were averaged/reported from the best rerun setting described in the final project report.

| Domain | Model | Validation accuracy | Test accuracy | Train accuracy |
|---|---:|---:|---:|---:|
| Graph | GCN | 0.2342 | 0.1754 | 0.4952 |
| Graph | GAT | 0.2432 | 0.1491 | 0.3600 |
| Graph | GIN | 0.2613 | 0.1579 | 0.1143 |
| Simplicial complex | SCCNN | 0.2523 | 0.1491 | 0.3810 |
| Cellular complex | CWN | **0.2703** | **0.2632** | **0.5886** |
| Hypergraph | AllSetTransformer | 0.2432 | 0.1579 | 0.4800 |
| Point cloud / set | DeepSet | 0.2252 | 0.1053 | 0.5272 |

CWN with CellCycle lifting achieved the highest test accuracy, 0.2632, compared with the best graph baseline, GCN, at 0.1754. This is about a 50% relative improvement over the best graph baseline, suggesting that the cellular-complex representation may capture useful higher-order structure in the neuronal correlation graphs.

The report also notes an important limitation: the dataset is small, with roughly 250 graph samples, so these numbers should be interpreted as an initial benchmark rather than a definitive performance claim.

## Upstream project

TopoBench is a framework for benchmarking topological deep learning models. The original upstream project is maintained by the Geometric Intelligence / Topological Intelligence ecosystem. This repository is a contribution snapshot/fork-style working copy used to show my A123-related implementation work.
