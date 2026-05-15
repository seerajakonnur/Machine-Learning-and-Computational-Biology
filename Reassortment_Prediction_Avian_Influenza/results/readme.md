# Results

This folder summarizes the key outputs from the reassortment prediction framework, including DNABERT-2 embedding visualization, Random Forest classifier performance, genetic algorithm recovery of reassortant candidates, and GAT-based segment interaction analysis.

## Embedding Space Analysis

t-SNE projection of the DNABERT-2-derived training embeddings revealed a clear separation between reassortant and non-reassortant genomes. This indicates that the segment-level genomic representations capture biologically meaningful reassortment-associated signals, even without task-specific fine-tuning.

## Random Forest Classifier

The Random Forest classifier achieved strong performance on unseen same-study test data.

| Metric | Result |
|---|---:|
| Test samples | 55 |
| Reassortants correctly identified | 30 / 30 |
| Non-reassortants correctly identified | 25 / 25 |
| Accuracy | 100% |
| Mean prediction confidence | 0.96 |
