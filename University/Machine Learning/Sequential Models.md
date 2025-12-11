---
Tags: 
Created: 2025-03-02 23:34:53
---
(Links:: [[Machine Learning]])
# Markov Models
# Deep learning on sequences
# Recurrent neural networks and LSTMs
# Transformers*

# Sequences in Machine Learning

## Introduction

Many real-world data types are naturally **sequential**, meaning that the order of the elements matters. Some examples include:

- **Text**: Words in a sentence follow grammatical rules.
- **Time Series**: Stock prices, weather data, and sensor readings evolve over time.
- **Biological Sequences**: DNA sequences encode genetic information.
- **Audio**: Speech signals contain patterns of frequency and duration.

Traditional machine learning models like linear regression or decision trees treat data as **independent and identically distributed (i.i.d.)**, meaning they ignore sequential dependencies. However, to process sequences effectively, we need models that can **capture dependencies over time**.

## Sequence Notation

A sequence is an **ordered list** of elements:

$$\mathbf{x} = (x_1, x_2, x_3, \dots, x_T)$$

where:

- $x_t$ represents the **element at time step $t$**.
- $T$ is the **length** of the sequence.
- Each $x_t$ can be a **scalar** (e.g., temperature reading), a **vector** (e.g., word embeddings), or even **structured data**.

We often work with **input-output sequences**, where an input sequence x\mathbf{x} produces an output sequence y\mathbf{y}:

$$\mathbf{x} = (x_1, x_2, ..., x_T) \quad \rightarrow \quad \mathbf{y} = (y_1, y_2, ..., y_T)$$

The length of $\mathbf{x}$ and $\mathbf{y}$ may be the same (e.g., language modeling) or different (e.g., machine translation).

---

## Types of Sequence Modeling

### 1. **One-to-One** (Standard ML)

- A fixed-length input produces a fixed-length output.
- Example: Traditional classification and regression.

### 2. **One-to-Many** (Sequence Generation)

- A single input generates a sequence.
- Example: Image captioning (image → sentence).

### 3. **Many-to-One** (Sequence Classification)

- A sequence is mapped to a single output.
- Example: Sentiment analysis (sentence → positive/negative).

### 4. **Many-to-Many (Same Length)**

- Every input element has a corresponding output.
- Example: Named entity recognition (word → label).

### 5. **Many-to-Many (Different Length)**

- The output sequence has a different length than the input.
- Example: Machine translation (English sentence → French sentence).

**Illustration of different sequence mappings:** ![Sequence Models](https://mlvu.github.io/sequences/sequence_models.svg)

---

## Sequence Dependence

Unlike standard ML problems, sequential data has **temporal dependencies**:

$$P(x_t \mid x_1, x_2, ..., x_{t-1})$$

This means that xtx_t is not independent but depends on previous values.

> [!note]- Markov Property  
> A **Markov process** assumes that the future state depends only on the **current state**, not the entire history:
> 
> $$P(x_t \mid x_{t-1}, x_{t-2}, ..., x_1) = P(x_t \mid x_{t-1})$$
> 
> This simplifies modeling but may lose important long-term dependencies.

For complex sequences, we need models that **capture both short-term and long-term dependencies**.

---

## Recurrent Neural Networks (RNNs)

A **Recurrent Neural Network (RNN)** processes sequential data by maintaining a **hidden state** hth_t that evolves over time:

$$h_t = f(W_h h_{t-1} + W_x x_t + b)$$

where:

- $h_t$ is the hidden state at time $t$.
- $x_t$ is the current input.
- $W_h$ and $W_x$ are weight matrices.
- $b$ is a bias term.
- $f$ is a non-linear activation function (e.g., tanh).

This allows information to **persist across time steps**.

### **Unfolding an RNN**

Instead of processing all inputs at once (like in traditional ML), an RNN **unfolds** across time:

![Unfolded RNN](https://mlvu.github.io/sequences/rnn_unfolded.svg)

Each step updates the hidden state:

$$h_1 \rightarrow h_2 \rightarrow h_3 \rightarrow ... \rightarrow h_T$$

This enables the model to **retain memory** of past inputs.

> [!summary] RNNs
> 
> - Can process sequences of arbitrary length.
> - Share weights across time steps.
> - Capture temporal dependencies.
> - Struggle with long-term dependencies due to **vanishing gradients**.

---

## Vanishing and Exploding Gradients

RNNs are trained using **Backpropagation Through Time (BPTT)**, but gradients can **vanish** or **explode** due to repeated multiplication:

- **Vanishing gradient**: Small gradients shrink exponentially → **early inputs lose influence**.
- **Exploding gradient**: Large gradients grow uncontrollably → **unstable training**.

To mitigate this:

- **Gradient clipping**: Limits maximum gradient values.
- **Long Short-Term Memory (LSTM) and Gated Recurrent Units (GRU)**: Use **gates** to regulate information flow.

---

## Long Short-Term Memory (LSTM)

An **LSTM** is an improved RNN that **controls memory updates** using **gates**:

- **Forget Gate**: Decides what to remove from memory.
- **Input Gate**: Decides what to add.
- **Output Gate**: Controls what to output.

LSTM equations:

$$f_t = \sigma(W_f h_{t-1} + U_f x_t + b_f)$$
$$i_t = \sigma(W_i h_{t-1} + U_i x_t + b_i)$$
$$\tilde{C}_t = \tanh(W_c h_{t-1} + U_c x_t + b_c) $$
$$C_t = f_t \odot C_{t-1} + i_t \odot \tilde{C}_t $$
$$o_t = \sigma(W_o h_{t-1} + U_o x_t + b_o) $$
$$h_t = o_t \odot \tanh(C_t)$$

where:

- $f_t$ is the **forget gate**.
- $i_t$ is the **input gate**.
- $o_t$ is the **output gate**.
- $C_t$ is the **cell state**.
- $h_t$ is the **hidden state**.

**LSTM Architecture:** ![LSTM Diagram](https://mlvu.github.io/sequences/lstm.svg)

> [!summary] LSTMs
> 
> - Overcome vanishing gradients.
> - Maintain memory over long sequences.
> - Use **gates** to selectively update information.

---

## Gated Recurrent Unit (GRU)

A **GRU** is a simplified LSTM with **two gates**:

- **Update Gate**: Controls how much past information to keep.
- **Reset Gate**: Controls how much to ignore.

GRU equations:

$$z_t = \sigma(W_z h_{t-1} + U_z x_t + b_z)$$
$$r_t = \sigma(W_r h_{t-1} + U_r x_t + b_r)$$
$$\tilde{h}_t = \tanh(W_h (r_t \odot h_{t-1}) + U_h x_t + b_h)$$
$$h_t = (1 - z_t) \odot h_{t-1} + z_t \odot \tilde{h}_t$$

GRUs are **faster** and use fewer parameters than LSTMs.

> [!summary] GRUs
> 
> - Similar to LSTMs but simpler.
> - Work well on many sequence tasks.
> - Often as effective as LSTMs.

---

## Conclusion

- Sequences are essential in **text, speech, finance, and biology**.
- **RNNs** capture **sequential dependencies**.
- **LSTMs and GRUs** solve vanishing gradient issues.
- Choosing between LSTM and GRU depends on **speed vs. complexity**.

For more details, visit [MLVU Sequences](https://mlvu.github.io/sequences/).

---
References: https://mlvu.github.io/sequences