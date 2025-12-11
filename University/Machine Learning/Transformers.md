---
Tags: 
Created: 2025-03-02 23:44:08
---
(Links:: [[Machine Learning]])
# 🤖 Self-Attention

## Overview
Self-attention is a mechanism that allows models to weigh the importance of different elements within a sequence when computing representationsThis mechanism is foundational to Transformer architectures, enabling models to process sequences in parallel and capture long-range dependencies

## Simple Self-Attention
In its simplest form, self-attention operates as follows
1. **Input Sequence**: A sequence of vectors $\{x_1, x_2, \ldots, x_n\}$
2. **Output Computation**: Each output vector $y_i$ is a weighted sum of all input vectors $$y_i = \sum_{j=1}^{n} \alpha_{ij} x_j$$ where $\alpha_{ij}$ represents the weight assigned to input $x_j$ when computing output $y_i$.
3. **Weight Calculation**: The weights $\alpha_{ij}$ are derived from the similarity between input vectors $x_i$ and $x_j$, typically using the dot product $$\alpha_{ij} = \text{softmax}(x_i \cdot x_j)$$
	1. The softmax function ensures that the weights are positive and sum to 1: $$\text{softmax}(z_i) = \frac{e^{z_i}}{\sum_{k=1}^{n} e^{z_k}}$$

## Practical Self-Attention

In practical implementations, self-attention incorporates additional features
- **Query, Key, and Value Vectors**:Each input vector xix_i is projected into three distinct vectors
    - **Query ($q_i$)**:( $q_i = W^Q x_i$
    - **Key ($k_i$)**:( $k_i = W^K x_i$
    - **Value ($v_i$)**:( $v_i = W^V x_i$
where $W^Q$, $W^K$, and $W^V$ are learned projection matrices

- **Scaled Dot-Product Attention**:The attention weights are computed using the scaled dot product of queries and keys $$\alpha_{ij} = \text{softmax}\left(\frac{q_i \cdot k_j}{\sqrt{d_k}}\right)$$ where $d_k$ is the dimensionality of the key vectors

- **Output Computation**:Each output vector $y_i$ is computed as $$ y_i = \sum_{j=1}^{n} \alpha_{ij} v_j $$

> [!note] **Key Points**
> -Self-attention enables models to focus on different parts of the input sequence when computing each output element
> -The use of queries, keys, and values allows for flexible and efficient representation learning

---

# 🏗️ Building Transformers

## Overview
Transformers are deep learning architectures that rely entirely on self-attention mechanisms, dispensing with recurrent and convolutional layer. This design allows for efficient parallel processing and has proven highly effective in modeling sequential dat.

## Encoder-Decoder Structure

A standard Transformer consists of two main component:
1. **Encoder** Processes the input sequence and generates a contextualized representatio.
2. **Decoder** Utilizes the encoder's representation to produce the output sequenc.

## Encoder Architecture

Each encoder layer comprise:
- **Multi-Head Self-Attention** Allows the model to jointly attend to information from different representation subspace.
- **Feed-Forward Neural Network** Applies a position-wise fully connected network to each position separately and identicall.
- **Residual Connections and Layer Normalization** Help stabilize and accelerate trainin.

## Decoder Architecture

Each decoder layer include:
- **Masked Multi-Head Self-Attention** Prevents each position from attending to subsequent positions, ensuring the autoregressive propert.
- **Encoder-Decoder Attention** Allows the decoder to focus on relevant parts of the input sequenc.
- **Feed-Forward Neural Network** Similar to the encoder's feed-forward networ.
- **Residual Connections and Layer Normalization** As in the encoder, these mechanisms aid in trainin.

> [!tip] **Advantages of Transformers**
> Enable parallel processing of sequence data, leading to faster training time.
> Capture long-range dependencies more effectively than traditional RNN.

---

# 💬 Building Chatbots

## Overvide

Transformers have revolutionized the development of chatbots by enabling more coherent and contextually appropriate responss Models like ChatGPT leverage Transformer architectures to engage in human-like conversatios.

## Key Components

- *_Pretraining_: The model is trained on large text corpora to learn language patterns and representatios.
    
- *_Fine-Tuning_: The pretrained model is further trained on domain-specific data to specialize in particular tasks or industris.
    
- **Reinforcement Learning from Human Feedback (RLHF)*: The model learns to generate more accurate and contextually appropriate responses through feedback loos.
    

## Benefits

---
References: https://mlvu.github.io/transformers