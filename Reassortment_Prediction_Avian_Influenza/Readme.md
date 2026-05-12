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

This project uses Influenza A H5N1 genome sequences curated from published studies, with a focus on reassortant and non-reassortant viruses.

### Training and Same-Clade Evaluation Dataset

Initial model development focused on H5N1 clade 2.3.4.4b sequences from the United States 2021–2022 outbreak period. The dataset includes both non-reassortant and reassortant genotypes.

| Dataset component | Description |
|---|---|
| Training non-reassortants | Genotype A1 sequences |
| Same-clade validation / test non-reassortants | Genotypes A2, A3, and later A4 sequences |
| Training reassortants | Major reassortant genotypes including B1.1, B1.2, B2, B3.1, B3.2, B4, and B5 |
| Same-clade test reassortants | Minor reassortant genotypes |

### Model Development Dataset

| Dataset component | Clade / background | Genotype(s) | Class | Use |
|---|---|---|---|---|
| Training non-reassortants | H5N1 clade 2.3.4.4b | A1 | Non-reassortant | Training |
| Training reassortants | H5N1 clade 2.3.4.4b | B1.1, B1.2, B2, B3.1, B3.2, B4, B5 | Reassortant | Training |

This setup enabled evaluation of whether the model could distinguish reassortant from non-reassortant genomes within the same broad clade background.

### Model Development Dataset

| Dataset component | Clade / background | Genotype(s) | Class | Number of sequences | Use |
|---|---|---|---|---:|---|
| Training non-reassortants | H5N1 clade 2.3.4.4b | A1 | Non-reassortant | 120 | Training |
| Training reassortants | H5N1 clade 2.3.4.4b | B1.1, B1.2, B2, B3.1, B3.2, B4, B5 | Reassortant | 119 | Training |
| **Total** | — | — | — | **239** | Training |

### Cross-Clade Evaluation Dataset

To assess generalization beyond the original training clade, an independent cross-clade dataset was curated from multiple published studies after the NeurIPS submission. This dataset was designed to include H5N1 reassortant and non-reassortant sequences from clades outside the primary 2.3.4.4b training setting.

The cross-clade evaluation dataset included:

| Source / group | Type | Clade / genotype details | Number of sequences |
|---|---|---|---:|
| Alaska 2022 dataset | Non-reassortant | A4 genotype, clade 2.3.4.4b | 6 |
| Bangladesh H5N1 clade 2.3.2.1a study | Non-reassortant | Clade 2.3.2.1a | 1 |
| Vietnam clade distribution study | Non-reassortant | VN3, clade 1.1.2 | 7 |
| Vietnam clade distribution study | Non-reassortant | VN12, clade 2.3.2.1a | 2 |
| Vietnam clade distribution study | Non-reassortant | VN45, clade 2.3.2.1b | 1 |
| Bangladesh H5N1 study | Reassortant | H5N1 clade 2.3.2.1a reassorted with low-pathogenic Eurasian-lineage avian influenza A viruses | 5 |

Overall, the cross-clade evaluation set contained **17 non-reassortant sequences** and **5 reassortant sequences**. Only sequences with complete CDS information were included where possible, and care was taken to avoid overlap with training data.


