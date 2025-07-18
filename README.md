# Percolation Clustering for Mixed-Type Data

## Project Overview

Percolation Clustering implements a scalable, interpretable pipeline for hierarchical clustering of datasets containing both numerical and categorical attributes. The method constructs a sparse, mixed-type neighborhood graph, selects high-centrality seed nodes, and grows clusters via edge-weight percolation under distance and categorical-match constraints. Initial components are merged by overlap criteria and refined with Ward linkage to produce a full dendrogram, whose optimal cut can be selected by internal validation indices.

## Key Features

- Mixed-type graph construction via Gower distance (single weighted graph) or layered Euclidean subgraphs  
- Centrality-driven seed selection (betweenness or closeness) to focus on structurally important nodes  
- Percolation-based cluster growth enforcing both distance threshold and minimum categorical-feature matches  
- Overlap-based merging of percolated components followed by Ward linkage on centroids for hierarchical refinement  
- No need to predefine the number of clusters; optimal cut can be chosen by cophenetic correlation, Silhouette score, or other metrics  
- Near-linear runtime in practice through sparse graph representation and localized depth-first search  
- Visualization tools for percolation stages and final dendrogram  

## Installation

Requirements:

  • Python 3.7 or higher  
  • numpy  
  • pandas  
  • scikit-learn  
  • networkx  
  • scipy  

Clone the repository and install dependencies:

```bash
git clone https://github.com/SmartGridandCity/Percolation_clustering.git
cd Percolation_clustering
pip install -r requirements.txt

# Command-Line Usage

A CLI entry point (run_clustering.py) accepts a CSV input and clustering parameters:

python run_clustering.py \
  --input data/mixed_data.csv \
  --numerical sepal_length,sepal_width,petal_length,petal_width \
  --categorical species \
  --graph_method gower \
  --seed_centrality betweenness \
  --seed_fraction 0.2 \
  --percolation_threshold 0.25 \
  --overlap_threshold 0.8 \
  --min_categorical_matches 1 \
  --output_dir results/

Output:

• clusters.csv — cluster assignments per sample
• dendrogram.png — hierarchical tree visualization
• metrics.json — Silhouette, Calinski–Harabasz, Dunn and Davies–Bouldin indices

# Python API

The core class PercolationClusterer is exposed in src/pipeline.py:

from src.pipeline import PercolationClusterer
import pandas as pd

df = pd.read_csv('data/mixed_data.csv')
clusterer = PercolationClusterer(
    numerical_features=['sepal_length','sepal_width','petal_length','petal_width'],
    categorical_features=['species'],
    graph_method='gower',
    seed_centrality='closeness',
    seed_fraction=0.1,
    percolation_threshold=0.2,
    overlap_threshold=0.85,
    min_categorical_matches=1
)
labels, dendro = clusterer.fit_predict(df)

# Repository Structure

Percolation_clustering/
├── data/                   Example and benchmark datasets  
├── notebooks/              Jupyter notebooks for exploration  
├── results/                Example outputs (clusters, plots, metrics)  
└── README.md               This file  

# Examples and Notebooks

The notebooks/ directory contains step-by-step demos on:

• Iris
• Titanic
• Palmer Penguins

Each notebook illustrates parameter tuning, percolation growth visualization, and dendrogram analysis.
