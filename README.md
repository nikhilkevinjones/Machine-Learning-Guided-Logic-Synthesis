# Machine Learning Guided Logic Synthesis -  Research Project

Logic Synthesis involves transforming high-level behavioral descriptions into detailed gate-level representations. With the remarkable success of Machine Learning (ML) in solving complex combinatorial and graph-related problems in diverse domains, there is a rising enthusiasm for the development of ML-guided logic synthesis tools. In this work, we introduce OpenABC-D, a comprehensive dataset created through the synthesis of open-source designs. This dataset is meticulously labeled and comprises 870,000 And-Inverter-Graphs (AIGs), generated from 1500 synthesis runs. The dataset includes both intermediate and final outputs and comes with associated labels, including optimized node counts and delay information. Here we predict the Quality of Result (QoR) as the normalized number of AIG nodes remaining after applying a series of synthesis transformation steps.

## Model Architecture
The model for the ML task for logic synthesis uses supervised learning to predict the quality of the synthesis result. We consider a simple architecture as shown in the figure below, which comprises of:
![arch](https://github.com/nikhilkevinjones/Machine-Learning-Guided-Logic-Synthesis/blob/main/archRP.png)
1. Inputs - which includes the AIG encoding representing the netlist and the synthesis recipe encoding which represent the sequence of synthesis transformations appied to the AIG.
2. Graph Convolution Network (GCN) - where the AIG graph is processed through a two-layer GCN. These GCN layers learn the node-level embeddings which aim to represent the connectivity patterns within the AIG.
3. Readout Mechanism - where two readout mechanisms global max pooling and average pooling generate graph-level embeddings. These embeddings compresses all the information of the graph into a vector, which contains essential properties of the graph’s structure.
4. Synthesis Recipe Encoding - where the recipe is passed through a linear layer and 1D convolution layer to capture sequential patterns in the synthesis recipe.
5. Concatenation - where the graph-level embedding (from readout) and synthesis recipe embedding (from convolution layer) are combined.
6. Regression - where the concatenated embeddings are passed through a series of fully connected (FC) layers. These layers perform regression to predict QoR values such as area and delay.
7. Output - is the final predicted QoR metric

## Predicting QoR
Predicting QoR of unseen synthesis recipe:
– Training Data: Utilizes IP netlists and the AIG (And-Inverter Graph)outputs of 1000 synthesis recipes used on those netlists (total of 29000 samples) for training.
– Prediction Task: Given an IP and a synthesis recipe (not seen during training), predicts the quality of the synthesis result, specifically the number of nodes in the AIG after synthesis.
– Use Case: The model predicts the quality of synthesis results for new, unseen 500 recipes, enabling the selection of the best recipe without running synthesis for all options, which can be time-consuming.

## Results
The runnable code for QoR prediction is done. Navigate to file "train2.py" by following the path - //OpenABC/models/qor/SynthNetV3/train2.py

The training is performed on NVIDIA GeForce RTX 2080 Graphics Processing Unit (GPU). The loss function used is Mean Square Error (MSE) to test the model’s effectiveness. Adam optimizer is used to adjust the model’s weights during training. Adam is an adaptive learning rate optimization algorithm that adjusts the learning rates of each parameter individually. The learning rate is 0.001, batch size is 4 and a total of 20 epochs being trained. The task is implemented with Python 3.9 using machine learning framework PyTorch 1.9 and graph neural network framework PyTorch-Geometric 1.7.

Training Loss 

![trainloss](https://github.com/nikhilkevinjones/Machine-Learning-Guided-Logic-Synthesis/blob/main/trainingloss.png)

Validation Accuracy

![val](https://github.com/nikhilkevinjones/Machine-Learning-Guided-Logic-Synthesis/blob/main/validationacc.png)
