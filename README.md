# Final-Year-Project-FYP
Hardware Trojan Detection using Graph Neural Networks (GNNs).

This project presents a Graph Neural Network (GNN)-based framework for Hardware Trojan detection in digital hardware designs. The proposed system analyzes hardware designs at the Register Transfer Level (RTL) and automatically identifies whether a circuit is Trojan-infected or Trojan-free.

The main objective is to develop a golden reference-free, automated, and scalable pre-silicon Hardware Trojan detection approach. Instead of relying on conventional simulation-based testing or manual inspection, the proposed method learns structural patterns directly from hardware designs using graph-based deep learning.

## Problem Statement

Hardware Trojans are malicious modifications inserted into integrated circuits during the design or manufacturing process. These modifications may remain hidden during normal operation and can compromise the security, functionality, or confidentiality of a hardware system.

Traditional Hardware Trojan detection approaches have several limitations:

- Simulation-Based Testing: Requires extensive test vectors and may fail to activate rare Trojan conditions.
- Side-Channel Analysis: Requires physical hardware and is generally performed after fabrication.
- Formal Verification: Can become computationally expensive for large and complex circuits and may require a golden reference design.
- Feature-Based Machine Learning: Often depends on manually extracted features and may fail to capture complex structural relationships within circuits.

Therefore, there is a need for an automated approach capable of learning hardware structural patterns directly from RTL designs.

## Project Objectives

The objectives of this project are:

- Develop an automated Hardware Trojan detection system using Graph Neural Networks.
- Analyze Verilog RTL designs without depending on a golden reference model.
- Convert hardware designs into Data Flow Graphs (DFGs).
- Extract graph-based structural information from hardware circuits.
- Train a Graph Convolutional Network (GCN) to distinguish between Trojan-infected and Trojan-free designs.
- Evaluate the model using Trust-Hub benchmark circuits.
- Test the generalization capability of the trained model on unseen hardware designs.


 ## Proposed Methodology
 
The complete Hardware Trojan detection pipeline is shown below:

┌─────────────────────┐
│    Verilog RTL      │
│    Hardware Code    │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│   PyVerilog Parser  │
│ RTL & Data Analysis │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│   DFG Generation    │
│ Nodes and Edges     │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│    Node Encoding    │
│ Graph Features      │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│    GCN Training     │
│ Pattern Learning    │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Trojan Classification│
│ TROJAN / NON-TROJAN │
└─────────────────────┘

## GNN Model Development

A Graph Convolutional Network (GCN) is used as the main classification model.

The GCN processes the graph structure and learns relationships between connected hardware components.

The model learns to identify structural differences between:

- Trojan-free hardware designs
- Trojan-infected hardware designs

Graph Neural Networks are particularly suitable for this problem because hardware circuits naturally contain complex relationships that can be represented as graphs.


## Model Training and Validation

The model was trained using the prepared graph dataset.
To evaluate its reliability, 5-fold cross-validation was performed.
The dataset was divided into five different folds, allowing the model to be trained and validated multiple times using different data partitions.
This provides a more reliable evaluation of the model's performance.

## Tools and Technologies

**Tool**                                        **Framework	Purpose**

Python	                                        Main programming language
Verilog                                        	Hardware design representation
HW2VEC	                                        Hardware-to-graph processing framework
PyVerilog	                                      Verilog parsing and data-flow analysis
NetworkX	                                      Graph processing and analysis
PyTorch	                                        Deep learning framework
PyTorch Geometric	                              Graph Neural Network implementation
Graph Convolutional Network (GCN)	              Hardware Trojan classification
Trust-Hub                                      	Hardware Trojan benchmark dataset


## Complete Project Workflow

Trust-Hub Hardware Circuits │ ▼ Dataset Preparation │ ▼ Verilog RTL Files │ ▼ PyVerilog Parsing │ ▼ Data Flow Graph (DFG) │ ▼ Feature Extraction / Encoding │ ▼ PyTorch Geometric Graph Data │ ▼ Graph Convolutional Network │ ▼ Model Training │ ▼ 5-Fold Cross-Validation │ ▼ Unseen Circuit Evaluation │ ▼ TROJAN / TROJAN-FREE Classification





