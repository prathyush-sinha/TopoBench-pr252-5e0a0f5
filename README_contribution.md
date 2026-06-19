# A123 Auditory Cortex Contribution Notes

This repository is a TopoBench snapshot used to document my contribution to the A123 mouse auditory cortex graph-construction and tutorial work.

## My contribution

I implemented A123 auditory cortex graph construction by pooling neurons across all five cortical layers for each `(session, best-frequency bin)` pair. The implementation collects neuron indices from every cortical layer, deduplicates them, slices the shared signal/noise correlation matrices, and emits one graph-level sample per frequency bin.

I also added tutorial coverage for training TopoBench/TBModel on the A123 auditory cortex dataset, including a CWN + TopoModelX example for graph-level classification.

## Files changed

- `topobench/data/datasets/a123.py` — A123 graph-construction logic and PyTorch Geometric `Data` conversion.
- `tutorials/tutorial_train_brain_model.ipynb` — end-to-end A123 training tutorial.
- `tutorials/tutorial_train_brain_model_CWN.ipynb` — CWN/TopoModelX tutorial for A123 graph-level classification.

Related pull request: [PR #1](https://github.com/prathyush-sinha/TopoBench-pr252-5e0a0f5/pull/1)

## Technical summary

### Dataset

- Source domain: mouse auditory cortex calcium-imaging data from the A123/Bowen et al. dataset.
- Processed representation: correlation-thresholded graphs.
- Node features: summary statistics from signal/noise correlation matrices.
- Label: best-frequency bin (`bf_bin`) for graph-level classification.

### Graph construction

The important implementation change is that graphs are constructed at the session/frequency-bin level rather than as separate per-layer graphs. For each frequency bin, the code pools tuned neurons across all five cortical layers before slicing the session-level correlation matrices. This creates a whole-session graph for that frequency bin and avoids carrying stale per-layer metadata in the generated PyG objects.

### Model/tutorial work

The CWN tutorial demonstrates how to use:

- `A123DatasetLoader`
- `PreProcessor`
- `CellCycleLifting`
- `TBModel`
- `CWN` from TopoModelX
- `CWNWrapper`
- `PropagateSignalDown`
- TopoBench loss, optimizer, and evaluator components

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

## Portfolio interpretation

This repo should be read as evidence of hands-on work with graph construction, neurodata preprocessing, topological deep learning tooling, TopoBench/TopoModelX model integration, and cross-domain benchmarking. The strongest contribution is the A123 graph-construction logic plus the CWN tutorial that shows how the processed graphs can be used in a topological neural network workflow.
