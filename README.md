UbGraph

Integrating Protein Language Models and Heterogeneous Biological Networks for E3/DUB–Substrate Prediction

UbGraph is a computational framework for predicting human E3 ubiquitin ligase–substrate and deubiquitinase (DUB)–substrate interactions by integrating protein language model representations with heterogeneous biological networks.

The final model-ready dataset contains 6,196 enzyme–substrate pairs involving 2,105 unique proteins. It comprises 3,806 experimentally supported positive interactions and 2,390 biologically filtered candidate-negative interactions.

Authors

* Shayan Aryania
* Hamid R. Kalhor

Repository Structure

* scripts/: Jupyter notebooks implementing the complete analysis pipeline
* src/: reusable source-code modules
* Data_proc/: processed interaction tables and quality-control reports
* Data_ml/: selected model inputs, predictions, evaluation metrics, and final outputs
* results/: selected result tables and figures
* environment-sequence.yml: Conda environment for data processing, protein language models, classical machine learning, and neural pair scoring
* environment-gnn.yml: Conda environment for graph learning, explainability, and downstream analyses

Large raw datasets, per-residue protein embeddings, pair-feature matrices, neural-network checkpoints, and complete candidate-space files are not stored directly in this GitHub repository.

Software Environments

Two Conda environments were used because the sequence-based and graph-based stages required different software configurations.

Sequence and Tabular Modeling Environment

Create the environment using:

conda env create -f environment-sequence.yml
conda activate ubgraph-sequence

This environment was used for:

* interaction-data processing and quality control
* candidate-negative construction and biological filtering
* protein-sequence retrieval
* protein language model embedding generation
* pair-feature construction
* classical machine-learning benchmarks
* XGBoost modeling
* neural pair scoring

The principal packages include:

* Python 3.13.13
* pandas 2.3.1
* NumPy 2.3.2
* scikit-learn 1.7.1
* XGBoost 3.0.4
* PyTorch 2.8.0
* Transformers 4.55.3
* fair-esm 2.0.0
* Biopython 1.87
* PyArrow 21.0.0
* Matplotlib 3.11.0

Graph-Learning Environment

Create the graph-learning environment using:

conda env create -f environment-gnn.yml
conda activate ubgraph-gnn

The final graph experiments used:

* Python 3.11.15
* PyTorch 2.12.0
* PyTorch Geometric 2.8.0
* pandas 3.0.3
* NumPy 2.4.6
* scikit-learn 1.9.0
* PyArrow 24.0.0
* Matplotlib 3.11.0
* seaborn 0.13.2

This environment was used for:

* homogeneous graph neural networks
* heterogeneous graph neural networks
* graph-transformer models
* edge-aware graph modeling
* ensemble construction
* graph explainability
* candidate prioritization
* cancer-specific analyses
* generation of final manuscript figures

Running the Notebooks

All notebooks use paths relative to the repository root. Jupyter must therefore be started from the root directory of the cloned repository:

cd UbGraph
jupyter lab

The notebooks should be run in numerical order.

Stage 1: Dataset Construction and Biological Filtering

1. scripts/011_parse_ubibrowser_positives.ipynb
2. scripts/012_check_positives.ipynb
3. scripts/021_build_initial_negatives.ipynb
4. scripts/031_filter_negatives_by_ppi.ipynb
5. scripts/041_filter_negatives_by_localization.ipynb
6. scripts/061_scRNA co-expression_with_scRNA co-expression.ipynb
7. scripts/071_Pair_Level.ipynb
8. scripts/081_checking_fasta_coverege_and_building_fasta_index.ipynb

Use the ubgraph-sequence environment for this stage.

Stage 2: Protein Representations and Sequence-Based Modeling

9. scripts/082_generate_embeddings_all_models.ipynb
10. scripts/083_build_pair_features_all_models.ipynb
11. scripts/091_baseline_benchmark.ipynb
12. scripts/100-CrossModel_Fusion_Benchmark.ipynb
13. scripts/110-Neural_Pair_Scorer.ipynb

Use the ubgraph-sequence environment for this stage.

Stage 3: Graph Learning and Downstream Analyses

14. scripts/120_GNN.ipynb
15. scripts/130_Explainability.ipynb
16. scripts/140_Case Studies.ipynb
17. scripts/150_External Validation.ipynb
18. scripts/160ـNovel_Prediction_Discovery.ipynb
19. scripts/170_Novel_Prediction_Explainability.ipynb
20. scripts/180_Cancer_Specific_Analysis.ipynb
21. scripts/190_Paper_Figures.ipynb

Switch to the ubgraph-gnn environment before running this stage.

Primary Data Sources

The analysis uses information obtained from the following resources:

* UbiBrowser 2.0 for experimentally supported human E3–substrate and DUB–substrate interactions
* BioGRID release 4.4.248 for human physical protein–protein interactions
* UniProt for human protein sequences and subcellular-location annotations
* TISCH2 for colorectal- and liver-cancer single-cell expression data

Raw third-party source files are not redistributed in this repository. Users should obtain these files from the corresponding official resources and comply with their applicable terms of use.

UbiBrowser Data

The pipeline expects the experimentally supported human interaction files to be placed under:

Data_raw/UbiBrowser/Raw/

The expected source files are:

E3-substrate-interactions.txt
DUB-substrate-interactions.txt

BioGRID Data

The physical protein–protein interaction filtering stage used BioGRID release 4.4.248.

The expected source file is:

Data_raw/Biogrid/BIOGRID-ALL-4.4.248.tab2.txt

UniProt Data

UniProt was used to obtain human protein sequences and subcellular-location annotations.

The expected localization file is:

Data_raw/uniprot/uniprot_human_cc.tsv

Sequence files are processed under:

Data_raw/uniprot/uniprot_fasta/

TISCH2 Data

The colorectal cancer datasets used in this study were:

* EMTAB8107
* GSE108989
* GSE139555
* GSE146771-10X
* GSE146771-Smart-seq2
* GSE166555

The liver hepatocellular carcinoma datasets were:

* GSE140228-10X
* GSE140228-Smart-seq2
* GSE146115
* GSE146409
* GSE166635
* GSE179795
* GSE98638

The expected directories are:

Data_raw/tisch2/CRC/
Data_raw/tisch2/LIHC/

Processed Dataset

The principal model-ready interaction table is:

Data_proc/pairs/pairs_all_model_ready.csv

The final dataset contains:

Dataset component	Count
Positive interactions	3,806
Candidate-negative interactions	2,390
Total enzyme–substrate pairs	6,196
Unique protein nodes	2,105

Candidate-negative interactions were constructed using degree-aware sampling and sequentially filtered using BioGRID physical interactions, UniProt subcellular-localization annotations, and TISCH2 single-cell coexpression evidence.

Biological Graph

The principal graph files are:

Data_ml/graph_dataset/graph_nodes.csv
Data_ml/graph_dataset/interaction_edges_labeled.csv
Data_ml/graph_dataset/ppi_edges.csv
Data_ml/graph_dataset/colocalization_edges.csv
Data_ml/graph_dataset/graph_edges_all.csv
Data_ml/graph_dataset/node_features_esm650.npy

The graph contains:

Graph component	Count
Protein nodes	2,105
Labeled enzyme–substrate pairs	6,196
Physical PPI edges	60,723
Colocalization edges	193,159
Total stored graph edges	260,078
Node-feature dimension	1,280

Files Excluded from GitHub

The following large or externally sourced files are intentionally excluded from the GitHub repository:

Data_raw/
Data_interim/
Data_feat/
Data_ml/pair_features/
Data_ml/neural/checkpoints/
Data_ml/neural/datasets/
Data_ml/neural/predictions/

Large cross-model feature matrices, complete novel-candidate spaces, and model checkpoint files are also excluded.

Per-sequence and per-residue embeddings can be regenerated using:

scripts/082_generate_embeddings_all_models.ipynb

Pair-feature matrices can be regenerated using:

scripts/083_build_pair_features_all_models.ipynb

Reproducibility Notes

* Random seeds were fixed for negative sampling, fold assignment, model initialization, and supported stochastic training operations.
* Cross-validation was grouped at the enzyme level.
* Fold-specific predictions, labels, pair identifiers, group identifiers, and performance summaries are provided where applicable.
* Prediction tables were aligned using class-aware pair identifiers.
* All reported tables and figures were generated programmatically from stored result files.
* Large protein embeddings and pair-feature matrices can be regenerated using the corresponding notebooks.
* The graph-based results were obtained under the exploratory transductive evaluation protocol described in the manuscript.
* Under this protocol, the complete enzyme–substrate adjacency was retained during message passing, and held-out folds contributed to checkpoint selection.
* Direct comparisons between sequence-based and graph-based models should therefore be interpreted cautiously.
* The final weighted heterogeneous ensemble was selected post hoc from out-of-fold predictions and is not an independently validated final-test estimate.

Main Performance Result

The final weighted heterogeneous ensemble combined:

* HGT with weight 0.5
* Edge-Aware HeteroGNN with weight 0.3
* HeteroGraphSAGE with weight 0.2

Its mean five-fold performance was:

Metric	Mean ± SD
ROC-AUC	0.8716 ± 0.0192
PR-AUC	0.9195 ± 0.0160
F1-score	0.8327 ± 0.0239
Accuracy	0.7955 ± 0.0218
Precision	0.8346 ± 0.0308
Recall	0.8319 ± 0.0354
Brier score	0.1490 ± 0.0162

Citation

The complete article citation and DOI will be added after publication of the UbGraph manuscript:

UbGraph: Integrating Protein Language Models and Heterogeneous Biological Networks for E3/DUB–Substrate Prediction

Contact

For questions concerning the study, contact the corresponding author:

Hamid R. Kalhor
Biochemistry and Chemical Biology Research Laboratory
Chemistry Department
Sharif University of Technology, Tehran, Iran
Email: kalhor@sharif.edu
