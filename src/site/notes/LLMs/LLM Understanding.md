---
{"dg-publish":true,"permalink":"/ll-ms/llm-understanding/"}
---


Sources:
- Taken from Andrej Karapathy (_the goat_) deep dive into LLMs.
 ![[LLM-Deep-Dive.excalidraw]]

My Notes:

# Pre-Training

- Download and preprocess the data.
# Tokenization

- Process of converting text(string) into tokens (integers).
## Naive Approaches
- Using word level or character level mapping using LUTs.
### Problems with this approach:
- Word level:
	- Vocabulary size is huge.
	- Out of vocabulary words.
	- Rare words are not handled well.
- Character level:
	- Too many tokens.
	- Long sequences.
	- Doesn't capture semantics.
- Doesn't account for other languages.

## Byte Pair Encoding
- Iteratively replaces the most common pair of consecutive tokens with a new token.
- This allows the model to learn sub-word units and handle out-of-vocabulary words more effectively.
For example, the word "unhappiness" might be tokenized as "un", "happi", and "ness".
- This is a greedy algorithm and is not optimal ( but it is used in practice).
- The way I understand this handles out of vocabulary words and rare words well is because we humans also kind of do the same, suffix-prefix and relational understanding is captured in this kinda.


# Training the Model

- Training the neural net to adjust it to get the predictions as the stats from training sets.

# Visualizing an LLM

- These are transformer architecture visualizations.

<iframe src="https://bbycroft.net/llm" width="100%" height="600px"></iframe>


# Inferences

- The process of using the trained model to make predictions on new data.
- Iteratively keep predicting the next token until we reach the end of sentence token or stopping criteria is met.

# GPT-2 

## [The Original Paper](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf)
- The original paper that introduced the GPT-2 model.
- It was based on transformer architecture and it has 1.6B parameters.
- Context length was 1024 tokens.
- Trained on 100 B tokens.
- Current SOTA models are around 500~700B parameters for CoT, (DeepSeek-R1 is 670B). ( Gemini output token size is 64k, insane scaling man)


# Base Model
- Token level trained dataset simulator.
- Probabilistic model that predicts the next token given the previous tokens.
- **Regurigation:** will recite some stuff from what it remembers from training.
- Will need few shot prompting ( and from this using it's in-context understanding) it might act like an assistant.


# Post-Training

## InstructGPT ( [Original Paper](https://arxiv.org/pdf/2203.02155) )
- 68 page paper 🛐.
- Basically taught us how to actually make InstructGPT, by creating a dataset of human feedback.
- Trained the model to follow instructions and be more helpful. Like remove toxic features by including those examples and teaching the model to say no.

### Open Source Datasets Visualization ( This visualization is of UltraChat, it's popular but now that Models became good it is mostly synthetic data. )

<iframe src="https://atlas.nomic.ai/data/stingning/ultrachat-1/map" width="100%" height="600px"></iframe>

# Hallucinations , Memory , Awareness

## How to reduce hallucinations?
- People figured out that we can add examples in the dataset like I don't know the answer to that or I am not sure about that.
	- Now how do we even figure this out? -> You Generate Synthetic Questions using powerful Models, ask them multiple times, answer changes/gets it wrong, you add an example in the instruct to say it doesn't know.
	- So After this process of adding multiple instructions, model learns that when it is not sure it should say it doesn't know.
- We also teach to use tools like web search using special tokens. 


## Memory

!!! "Vague recollection" vs. "Working memory" !!!
- Knowledge in the parameters == Vague recollection (e.g. of something you read 1 month ago)
- Knowledge in the tokens of the context window == Working memory

## Knowledge of Self


# Transformer related Stuff


![Pasted image 20250522143616.png](/img/user/LLMs/Pasted%20image%2020250522143616.png)


## Self-Attention
### Mathematical Expression for Self-Attention
The self-attention mechanism is like THE core of transformer models that power LLMs. 

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

Where:
- $Q \in \mathbb{R}^{n \times d_k}$ represents the queries
- $K \in \mathbb{R}^{n \times d_k}$ represents the keys
- $V \in \mathbb{R}^{n \times d_v}$ represents the values
- $d_k$ is the dimension of the keys (used for scaling)
- $n$ is the sequence length

The attention weights:

```tikz
\begin{document}
\begin{tikzpicture}[scale=0.8]
\draw[->] (0,0) -- (4,0) node[right] {Token position (keys)};
\draw[->] (0,0) -- (0,4) node[above] {Token position (queries)};
\draw[very thin,color=gray] (0,0) grid (3,3);

\filldraw[fill=blue!20] (0,0) rectangle (1,1);
\filldraw[fill=blue!70] (1,0) rectangle (2,1);
\filldraw[fill=blue!30] (2,0) rectangle (3,1);

\filldraw[fill=blue!80] (0,1) rectangle (1,2);
\filldraw[fill=blue!10] (1,1) rectangle (2,2);
\filldraw[fill=blue!40] (2,1) rectangle (3,2);

\filldraw[fill=blue!20] (0,2) rectangle (1,3);
\filldraw[fill=blue!50] (1,2) rectangle (2,3);
\filldraw[fill=blue!90] (2,2) rectangle (3,3);

\node at (1.5,-0.5) {Attention Matrix};
\end{tikzpicture}
\end{document}
```

For multi-head attention, we have:

$$\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, \ldots, \text{head}_h)W^O$$

Where each head is computed as:

$$\text{head}_i = \text{Attention}(QW_i^Q, KW_i^K, VW_i^V)$$

With learnable projection matrices:
- $W_i^Q \in \mathbb{R}^{d_{\text{model}} \times d_k}$
- $W_i^K \in \mathbb{R}^{d_{\text{model}} \times d_k}$
- $W_i^V \in \mathbb{R}^{d_{\text{model}} \times d_v}$
- $W^O \in \mathbb{R}^{hd_v \times d_{\text{model}}}$

The loss function for next-token prediction is typically cross-entropy:

$$\mathcal{L} = -\sum_{i=1}^{n} \sum_{j=1}^{V} y_{i,j} \log(p_{i,j})$$

Where:
- $n$ is the number of tokens in the sequence
- $V$ is the vocabulary size
- $y_{i,j}$ is 1 if token $i$ is the $j$-th word in the vocabulary, 0 otherwise
- $p_{i,j}$ is the predicted probability that token $i$ is the $j$-th word


