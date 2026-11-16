# mini-qforest

**Forest detection on Sentinel-2 images with graph neural networks and hybrid quantum classifiers.**

A small, reproducible study that compares three families of models on the same task, recognising forest in satellite images:

1. **classical machine learning**: random forest on interpretable features, small CNN on raw images;
2. **graph neural networks**: images as nodes of a k-nearest-neighbour graph, GCN and GAT;
3. **hybrid quantum models**: a variational quantum classifier and a quantum kernel SVM on 4 qubits (simulated).

This project is a personal preparation for the PhD topic *"Hybrid quantum graph learning for old-growth forest detection"* (Q-FOREST project, IRIT / Université de Toulouse).

## Data

[EuroSAT](https://github.com/phelber/EuroSAT): 27,000 Sentinel-2 patches of 64 × 64 pixels in 10 land-cover classes, including *Forest*. RGB version, downloaded automatically by `torchvision`.

Two tasks: **forest vs rest** (binary) and **10 classes**. One stratified split (70 / 15 / 15) is shared by all notebooks, so every score is computed on the same test images.

## Notebooks

| Notebook | Content | Open |
| --- | --- | --- |
| `01_exploration_baselines.ipynb` | Exploration, 35 interpretable features (colour, Green Leaf Index, texture), random forest, small CNN | [Colab](https://colab.research.google.com/github/EliasMht/mini-qforest/blob/main/01_exploration_baselines.ipynb) |
| `02_graph_gnn.ipynb` | k-NN graph of images, homophily, MLP vs GCN vs GAT, sensitivity to k | [Colab](https://colab.research.google.com/github/EliasMht/mini-qforest/blob/main/02_graph_gnn.ipynb) |
| `03_hybrid_quantum.ipynb` | PCA to 4 qubits, variational quantum classifier, IQP quantum kernel, classical baselines of the same size | [Colab](https://colab.research.google.com/github/EliasMht/mini-qforest/blob/main/03_hybrid_quantum.ipynb) |

Run the notebooks in order: notebook 01 saves the features and the split to Google Drive (`MyDrive/mini-qforest/results`), and notebooks 02 and 03 reuse them.

## Results

*To be filled in after running the notebooks (from `results/results.csv`).*

| Model | Task | Accuracy | F1 | Parameters |
| --- | --- | --- | --- | --- |
| Random forest (35 features) | forest vs rest | | | – |
| Small CNN (raw images) | forest vs rest | | | ~95k |
| MLP (no graph) | forest vs rest | | | |
| GCN (k-NN graph, k = 10) | forest vs rest | | | |
| GAT (k-NN graph, k = 10) | forest vs rest | | | |
| VQC (4 qubits, 3 layers) | forest vs rest, 200 balanced test images | | | 37 |
| Quantum kernel SVM (IQP) | forest vs rest, 200 balanced test images | | | – |
| MLP 4-6-1 (same 4 inputs) | forest vs rest, 200 balanced test images | | | 37 |

![Comparison](results/comparison_quantum.png)

## Main findings

*Write 3 to 5 sentences: does the graph help, and do the quantum models match classical models of the same size?*

## Limitations

- EuroSAT patches have no geographic coordinates, so the graph is built from feature similarity, not from spatial adjacency.
- Quantum circuits are simulated without noise, on 4 qubits and a small balanced subsample.
- RGB only: with the 13-band version, the NDVI and other spectral indices would replace the Green Leaf Index.

## Next steps

- Spatial graphs: segment a full Sentinel-2 tile into parcels, connect adjacent parcels, classify with a GNN.
- Multimodal node features (optical, radar Sentinel-1, LiDAR canopy height).
- Quantum layers that follow the graph structure (QAOA-inspired), and runs on real hardware with noise.

## Installation (local)

```bash
pip install -r requirements.txt
jupyter notebook notebooks/
```

## Author

Adam Elias Mahamat — statistician engineer (ENSEA Abidjan), M2 TIDE, Université Paris 1 Panthéon-Sorbonne.
[LinkedIn](https://www.linkedin.com/in/adameliasmahamat/)

## License

MIT
