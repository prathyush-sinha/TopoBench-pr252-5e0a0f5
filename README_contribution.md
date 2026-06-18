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

## Results/status

This snapshot documents the data-processing and model-integration work. The tutorial notebooks are cleaned so they do not include local machine paths or captured execution output.

Current reproducibility status:

- Dataset/task: A123 auditory cortex graph-level classification.
- Model path: CWN/TopoModelX through TopoBench `TBModel`.
- What ran successfully: tutorial configuration and integration code were prepared for loading the A123 dataset, applying cell-complex lifting, constructing the CWN model stack, and running Lightning training.
- Metrics/screenshots/logs: no final benchmark metrics are committed in this repository snapshot. Add validated training logs or a metrics table before claiming model performance in a resume or recruiter discussion.

## Recruiter-facing interpretation

This repo should be read as evidence of hands-on work with graph construction, neurodata preprocessing, topological deep learning tooling, and TopoBench/TopoModelX model integration. The strongest contribution is the A123 graph-construction logic plus the CWN tutorial that shows how the processed graphs can be used in a topological neural network workflow.
