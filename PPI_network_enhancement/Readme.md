
# Host-Pathogen Protein Interaction Network Enhancement Using Recommender Systems and Machine Learning

This project predicts novel host-pathogen protein-protein interactions by combining a probabilistic spreading algorithm with supervised machine learning on amino-acid-derived protein features.
Starting from experimentally reported HIV-human binding interactions, the pipeline expands the interaction network, filters likely false positives, and prioritizes biologically plausible candidate interactions for downstream disease-mechanism analysis and potential drug-target discovery.

## Project Motivation

Host-pathogen protein-protein interactions are central to infection biology. They reveal how pathogens interact with host cellular machinery and can help identify proteins that may be useful as therapeutic targets.

## Overview

The workflow has three main stages:

1. Network-based candidate generation using a probabilistic spreading algorithm.
2. Biological feature extraction from protein motifs and amino acid properties.
3. Machine learning-based filtering using a Random Forest classifier.

## Data Acquisition:

• Downloaded the bipartite protein-protein interaction network from the NCBI database in .csv format.

•	The CSV file includes:

   1. Host and pathogen protein names
   2. Accession numbers
   3. Gene IDs
   4. Interaction types (e.g., binds, downregulates, affects, decreases, inhibits), dataset contains 147 distinct types of interactions
   5. Interaction description in detail



##  Initial Focus:
•	For the initial phase of the project, I focused solely on binding interactions. The reason being binding interactions are the actual physical inetactions between two proteins hence more reliable than any other types of interactions and thus serve a good starting point.

•	The objective was to use these binding interactions as a starting point to enhance the network.

•Then the interactions which we get after enehnacement can be cross checked and see if we they match with any other types of interactions that were there in the original data. This way we can validate the enhanced interactions.

![Binding Interactions - Starting point](binding_interactions_starting_point.pdf)


##  Network Enhancement:
•	We applied a novel technique to expand the interaction network.

•	The goal was to identify additional types of interactions present in the original dataset but not initially considered.

## Algorithm Application:
•	We applied a probabilistic spreading algorithm (PSA) commonly used in recommendation systems to the binding interactions.
•	The analysis was performed using the PSA.py script, which requires a tab-separated input file with two columns, where the first column contains pathogen proteins and the second column contains host proteins.

## Output:

* The `PSA.py` script outputs a `.txt` file.
* The output file contains:

  1. The pathogen protein in the first column.
  2. The corresponding interacting host protein along with the likelihood of interaction in the second column.
  3. The PSA algorithm generates a wide range of recommended interactions. However, since it does not consider biological properties, many of these interactions are likely to be false positives.
  4. To address this, it is crucial to filter the interactions and establish a cutoff for further analysis.
  5. To determine the appropriate cutoff, we plotted a histogram of the interaction likelihood values to examine their distribution. Based on the density of values, we selected only those interactions above the 75th percentile for further analysis. This approach helps ensure that the most likely interactions are prioritized while reducing the inclusion of potential false positives.
  6. We utilized machine learning to further filter the predicted interactions.



## Feature Selection:
•	Amino acid properties were used as features for the machine learning model.

•	Hypothesis: A set of host proteins interacting with one pathogen protein must share common features by virtue of which they are all interacting with the same pathogen protein.

•	Predicted proteins must also exhibit these similar features.

•	These features were exploited using machine learning techniques.

• These features were used in the machine learning model: charge, hydrophobic ratio, aromaticity, aliphatic index, instability Index, isoelectric point, boman index, kidera properties (10).

## Data Preparation:
•	Protein sequences for the relevant proteins were downloaded from UniProt.

•	The next processes was performed separately for each pathogen protein. Theese processes were completed for 5 pathogen proteins,  namely gag, matrix, nef, rev, vpr.

•	Grouping Host Proteins: For each pathogen protein (e.g. gag), three separate groups of human proteins were prepared:
1.	Proteins interacting with the pathogen protein.
2.	Proteins not interacting with the pathogen protein.
3.	Predicted interacting proteins generated from the PSA algorithm.

## Motif Conversion:
•	The protein sequences were converted to motifs using the MEME tool. Command used for motif conversion:

```bash
meme -mod anr protein_sequences.fasta -protein -o meme_output -nmotifs 5 
```


## Amino Acid Property Calculation:
•	Calculate the amino acid properties for all the motifs. The code attributes.py calculates charge, hydrophobic ratio, aromaticity, aliphatic index properties using the modlamp Python package. Instability Index, isoelectric point, boman index, kidera properties (10) are calculated using R codes boman_ii_ip.r and kidera.r

## Supervised Machine Learning:
• Algorithm Used: A supervised machine learning algorithm, specifically a Random Forest classifier, was used to classify the predicted proteins (from PSA) as interacting or non-interacting with a particular pathogen protein.

• Training Data: The labelled data from earlier preparations were used, where proteins were separated into interacting and non-interacting groups.

• Enhancement: A total of 35 new interactions were identified for 5 pathogen proteins using this technique.

## Network Visualisation:
• Cytoscape, an open source bioinfomartics software was used to visualize the interactions.

• The binding_interactions_starting_point.pdf file shows the interaction network containing only binding interactions, which serves as the starting point for this project.

• The enhanced2.pdf shows enhanced network where the new interactions are colored differently in shades of pink. The color intensity ranges from light to dark, indicating the reliability of the newly predicted interactions. Darker colors represent higher confidence levels. This only shows new interactions 5 pathogen proteins and not all.



