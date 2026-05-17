# Results

This folder summarizes the key outputs from the reassortment prediction framework, including DNABERT-2 embedding visualization, Random Forest classifier performance, genetic algorithm recovery of reassortant candidates, and GAT-based segment interaction analysis.

## Embedding Space Analysis

PCA and t-SNE were used to visualize the DNABERT-2-derived segment-specific embeddings from the training dataset.

The PCA projection shows a strong global separation between reassortant and non-reassortant genomes. Non-reassortants form a compact cluster, suggesting that genotype A1 non-reassortant genomes are relatively homogeneous in the learned embedding space. In contrast, reassortants separate into multiple visible subclusters, consistent with the presence of multiple reassortant genotypes and segment-combination patterns.

The t-SNE projection further emphasizes local neighborhood structure. Reassortant genomes remain clearly separated from non-reassortants and show distinct subclusters, supporting the idea that reassortants are not a single uniform group. Non-reassortants appear more spread out in t-SNE than in PCA, which is expected because t-SNE prioritizes local relationships and can expand compact global clusters to reveal finer-scale variation.

Together, PCA and t-SNE provide complementary views: PCA highlights the broad global separation between reassortant and non-reassortant genomes, while t-SNE reveals local structure and within-class heterogeneity. The consistent class-level separation across both methods suggests that DNABERT-2 segment-specific embeddings capture biologically meaningful reassortment-associated genomic patterns without task-specific fine-tuning.

![t-SNE visualization](../assets/influenza_clustering_segment_specific_pca.png)

![PCA visualization](../assets/influenza_clustering_segment_specific_tsne.png)


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

### Multi-Level Embedding Structure

This PCA plot visualizes DNABERT-2 embeddings using three biological metadata layers: shape indicates reassortment status, edge color indicates clade, and face color indicates grouped genotype. The embeddings show clear organization across clade, genotype, and reassortment labels, demonstrating that the learned representations capture multi-level biological structure rather than only a binary class signal.

The separation of external clades from the primary 2.3.4.4b group, together with genotype-level clustering within the same embedding space, suggests that DNABERT-2 segment-specific embeddings encode meaningful evolutionary and reassortment-associated variation.

![PCA visualization](../assets/pca_grouped_hierarchical.png)


## Genetic Algorithm Results

Because Influenza A contains eight independently exchangeable genome segments, the number of potential reassortant combinations increases rapidly with the number of parental viruses. The genetic algorithm (GA) module was used to prioritize biologically plausible reassortant candidates from this large combinatorial space.

The GA successfully recovered known reassortant genotype patterns from outbreak data, suggesting that the framework captures biologically meaningful segment compatibility constraints. 

### Genetic Algorithm Fitness Criteria

The genetic algorithm was designed to prioritize biologically plausible reassortant genomes by scoring candidate segment combinations using influenza-specific compatibility rules. Each candidate genome was represented as an 8-segment combination derived from two parental viruses.

The fitness function included the following criteria:

| Fitness criterion | Biological rationale | Fitness contribution |
|---|---|---:|
| Intact polymerase complex | PB2, PB1, and PA were rewarded when inherited from the same parent, reflecting functional compatibility of the influenza polymerase complex. | +100 |
| NP–polymerase compatibility | NP was rewarded when inherited from the same parent as the intact polymerase complex, reflecting the functional association between NP and polymerase activity. | +50 |
| Partial polymerase integrity | Candidates with two of the three polymerase segments from the same parent were given a smaller reward, representing partial compatibility. | +35 |
| HA–NA co-inheritance | HA and NA were rewarded when inherited from the same parent, reflecting coordinated surface protein compatibility. | +40 |
| Avoidance of fully parental genomes | Fully parental segment combinations were penalized to encourage true reassortant candidates rather than unchanged parental genomes. | −70 |

These biologically informed constraints guided the GA toward reassortant candidates that preserve key functional relationships while still exploring novel segment combinations.
![GA](../assets/GA.png)
