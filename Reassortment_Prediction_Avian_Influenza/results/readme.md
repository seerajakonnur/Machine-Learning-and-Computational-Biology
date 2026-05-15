# Results

This folder summarizes the key outputs from the reassortment prediction framework, including DNABERT-2 embedding visualization, Random Forest classifier performance, genetic algorithm recovery of reassortant candidates, and GAT-based segment interaction analysis.

## Embedding Space Analysis

PCA and t-SNE were used to visualize the DNABERT-2-derived segment-specific embeddings from the training dataset.

The PCA projection shows a strong global separation between reassortant and non-reassortant genomes. Non-reassortants form a compact cluster, suggesting that genotype A1 non-reassortant genomes are relatively homogeneous in the learned embedding space. In contrast, reassortants separate into multiple visible subclusters, consistent with the presence of multiple reassortant genotypes and segment-combination patterns.

The t-SNE projection further emphasizes local neighborhood structure. Reassortant genomes remain clearly separated from non-reassortants and show distinct subclusters, supporting the idea that reassortants are not a single uniform group. Non-reassortants appear more spread out in t-SNE than in PCA, which is expected because t-SNE prioritizes local relationships and can expand compact global clusters to reveal finer-scale variation.

Together, PCA and t-SNE provide complementary views: PCA highlights the broad global separation between reassortant and non-reassortant genomes, while t-SNE reveals local structure and within-class heterogeneity. The consistent class-level separation across both methods suggests that DNABERT-2 segment-specific embeddings capture biologically meaningful reassortment-associated genomic patterns without task-specific fine-tuning.

![t-SNE visualization](../assets/influenza_clustering_segment_specific_pca.png)

![t-SNE visualization](../assets/influenza_clustering_segment_specific_tsne.png)


## Random Forest Classifier

The Random Forest classifier achieved strong performance on unseen same-study test data.

| Metric | Result |
|---|---:|
| Test samples | 55 |
| Reassortants correctly identified | 30 / 30 |
| Non-reassortants correctly identified | 25 / 25 |
| Accuracy | 100% |
| Mean prediction confidence | 0.96 |

## External-Study Evaluation

The Random Forest classifier was also evaluated on an independently curated external-study dataset containing **17 non-reassortants** and **5 reassortants** from multiple published H5N1 studies. This evaluation tested how well the model generalized beyond the original same-study test setting.

| Metric | Result |
|---|---:|
| Test samples | 22 |
| Non-reassortants correctly identified | 14 / 17 |
| Reassortants correctly identified | 5 / 5 |
| Accuracy | 86.36% |
| Balanced Accuracy | 91.18% |
| MCC | 0.7174 |

The model correctly identified all reassortant samples in this external-study test set, while 3 non-reassortant samples were misclassified as reassortants. Despite the small and imbalanced dataset, the MCC of 0.7174 indicates useful generalization across independently sourced sequences.
