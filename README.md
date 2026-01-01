# AI for AML Compliance using Graph Analytics

Copyright (c) 2026 Shrikara Kaudambady. All rights reserved.

## 1. Introduction

Anti-Money Laundering (AML) compliance is a critical function in the finance sector. Traditional AML systems often rely on simple, rule-based flags (e.g., transactions over a certain amount), which can miss sophisticated laundering schemes that involve complex webs of accounts and transactions.

This project demonstrates a modern, AI-powered approach using **Graph Analytics**. By modeling the financial network as a graph—where accounts are nodes and transactions are edges—we can use powerful algorithms to uncover suspicious structural patterns and relationships that are invisible to transaction-level analysis.

## 2. The Solution Explained: Graph Features + Anomaly Detection

This solution identifies suspicious accounts by first analyzing their role and behavior within the financial network and then using an anomaly detection model to flag the outliers.

### 2.1 Modeling Transactions as a Graph

We use the `networkx` library to construct a directed graph from a list of transactions. This allows us to apply graph theory concepts to our problem. In our simulation, we deliberately create a small, tightly-knit cluster of accounts that represent a potential money laundering "ring."

### 2.2 Graph-Based Feature Engineering

Instead of looking at just transaction amounts, we extract features for each account based on its position and activity within the graph. These features describe the *behavior* of the account:

*   **Degree Centrality:** The total number of incoming and outgoing transactions. A very high degree might indicate a "hub" or "money mule" account.
*   **Clustering Coefficient:** A measure of how tightly connected an account's neighbors are to each other. An unusually high clustering coefficient can indicate a "smurfing" ring where money is circulated within a small, closed group of accounts to obscure its origin.
*   **PageRank:** An algorithm (famously used by Google) to measure the "importance" of a node in a network. An account with an unusually high PageRank might be a central player in a laundering scheme.

### 2.3 Anomaly Detection with Isolation Forest

Once we have computed these graph-based features for every account, we have a structured dataset describing their network behavior. We then use the **`IsolationForest`** algorithm from `scikit-learn` to perform unsupervised anomaly detection.

The model learns what the feature profile of a "normal" account looks like and then effectively flags the accounts whose combination of degree, clustering, and importance is most unusual. This approach successfully identifies the members of our simulated laundering ring, which a simple rule-based system would likely miss.

The final output is a list of suspicious accounts and a powerful visualization of the transaction graph, with the detected anomalies clearly highlighted.

## 3. How to Use the Notebook

### 3.1. Prerequisites

This project requires `networkx` for graph analysis, along with standard data science libraries.

```bash
pip install pandas numpy scikit-learn networkx matplotlib seaborn
```

### 3.2. Running the Notebook

1.  Clone this repository:
    ```bash
    git clone https://github.com/shrikarak/aml-compliance-ai-graph-analysis.git
    cd aml-compliance-ai-graph-analysis
    ```
2.  Start the Jupyter server:
    ```bash
    jupyter notebook
    ```
3.  Open `aml_graph_detector.ipynb` and run the cells sequentially. The notebook will simulate the data, build the graph, train the model, and generate a visualization of the results.

## 4. Deployment and Customization

This notebook provides a powerful template for a real-world AML detection system.

1.  **Use Real Transaction Data:** The data simulation cell can be replaced with a data loader that pulls transaction records from a live database or data warehouse.
2.  **Scale the Graph:** For millions of accounts, `networkx` may be slow. A production system would use a more scalable graph analytics platform like **Apache Spark's GraphFrames** or a dedicated graph database like **Neo4j**. The core logic of feature extraction and anomaly detection would remain the same.
3.  **Enhance Features:** The model can be made more powerful by adding more features to the graph nodes (e.g., account age, customer type, geographic location) and edges (e.g., transaction amount, time of day).
4.  **Graph Neural Networks (GNNs):** For a state-of-the-art approach, the entire feature engineering and anomaly detection process can be replaced by a Graph Neural Network. A GNN could learn the features automatically and might uncover even more subtle and complex laundering patterns.
