# Predicting Reassortment potential in Influenza A virus using foundation models (DNABERT2) and genetic algorithms

## Overview
This project presents an AI-driven framework for predicting Influenza A reassortment potential using foundation-model-derived genomic representations. Influenza genomes are processed segment-wise using DNABERT-2 embeddings, followed by a Random Forest classifier for reassortment prediction. 

In addition to classification, a Graph Attention Network (GAT) is used to model segment-level relationships and characterize attention-guided segment compatibility patterns associated with reassortment. A genetic algorithm module is used to explore biologically plausible reassortant candidates.

This work was accepted at the NeurIPS 2025 2nd Workshop on Foundation Models for Life Sciences (FM4LS).

![Alt text](assets/to_use_reassortment_image.png)

## Why Reassortment Prediction Matters

Influenza A viruses have segmented genomes, which allows whole genome segments to be exchanged when two viruses co-infect the same host. This process, known as reassortment enables rapid viral evolution and generate novel viral genotypes with altered host range, transmissibility, or pandemic potential.

Early identification of reassortment signals is important for genomic surveillance, outbreak preparedness, and prioritizing high-risk viral combinations for downstream investigation.

## Methodology

The framework consists of four main components:

### 1. Segment-wise embedding generation

Each Influenza A genome is represented using its 8 genomic segments: PB2, PB1, PA, HA, NP, NA, MP, and NS. Each segment is processed independently through a foundation model (DNABERT-2) to generate segment-level embeddings. These embeddings are then concatenated to form a genome-level representation.

### 2. Reassortment classification

A Random Forest classifier is trained on concatenated DNABERT-2 segment embeddings to classify genomes as reassortant or non-reassortant.

### 3. Segment interaction analysis with GAT

Beyond the primary Random Forest classifier, a Graph Attention Network (GAT) is used to model segment-level relationships within each influenza genome. Each genome is represented as an 8-node graph corresponding to PB2, PB1, PA, HA, NP, NA, MP, and NS, enabling attention-guided characterization of segment compatibility patterns associated with reassortment.

### 4. Genetic Algorithm-Based Reassortant Candidate Generation

Influenza virus reassortment is not entirely random; it is shaped by factors such as host species, viral subtypes, and compatible segment combinations. The goal is to simulate reassortment by generating multiple potential reassortant genomes and evaluating them using the trained classifier to determine whether they qualify as reassortants or non-reassortants. 

## Dataset

Initial model development used H5N1 clade 2.3.4.4b sequences from the United States 2021–2022 outbreak period, which was marked by extensive reassortment.

The dataset includes:

- Non-reassortant genotypes: A1, A2, A3
- Reassortant genotypes: B1.1, B1.2, B2, B3.1, B3.2, B4, B5
- Minor reassortant genotypes reserved for testing

For model development:

| Dataset | Composition |
|---|---|
| Training non-reassortants | 120 genotype A1 sequences |
| Training reassortants | 119 sequences from B1.1, B1.2, B2, B3.1, B3.2, B4, B5 |
| Same-clade test non-reassortants | 25 sequences from A2 and A3 |
| Same-clade test reassortants | 30 minor reassortant sequences |
| Cross-clade test | Independent reassortant and non-reassortant sequences from additional clades |



## Data
For generating the initial results, H5N1 clade 2.3.4.4b sequences from the United States (2021–2022)—a period marked by major reassortment events were used. This dataset includes both non-reassortant and reassortant genotypes circulating during this time. Specifically, it contains non-reassortant genotypes such as A1, A2, and A3, along with reassortant genotypes including B1.1, B1.2, B2, B3.1, B3.2, B4, B5, and several minor reassortants that represent subsets of these major groups.
For model development, 120 non-reassortant sequences from genotype A1 were selected as the negative training set, and 119 reassortant sequences spanning genotypes B1.1, B1.2, B2, B3.1, B3.2, B4, and B5 were used as the positive training set, preserving proportional representation across classes. To evaluate model generalization on unseen data, non-reassortant genotypes A2 and A3 (25 sequences combined) were reserved exclusively for testing, while 30 minor reassortants served as the reassortant test set. 
