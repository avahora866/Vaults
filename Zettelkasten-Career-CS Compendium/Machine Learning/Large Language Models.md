---
tags:
  - "#TRUNK"
---

# The Atom of Language: Tokenization
Large Language Models (LLMs) do not process text as humans do; they require mathematical input in the form of integers. Before an LLM can understand a sentence, the raw text must be converted into numerical identifiers called **Tokens**.

## The Vocabulary (The Lookup Table)
Before an LLM is trained, a massive, static list of all the unique text chunks the model knows is defined. This is called the Vocabulary.
* It functions similarly to a dictionary or hash map (`Map<int, string>`), where the key is the Integer ID and the value is the text fragment (Token).
* For a model like GPT-4, the vocabulary size is roughly 100,000 unique tokens.
* A **Tokenizer** algorithm scans text and maps it to these predefined IDs.

## Sub-word Tokenization
Tokens are not strictly whole words.
* **Common words** (e.g., "apple") typically receive a single token.
* **Complex or rare words** are broken down into multiple sub-word tokens.
* **Code** is often broken down into very specific syntax-based tokens.

**Example:** The word "Tokenization" might be broken into two tokens by a standard tokenizer: `Token` (ID: 11450) and `ization` (ID: 1952). The input vector sent to the model is simply `[11450, 1952]`.

If the tokenizer encounters a completely novel word that did not exist in its training data, it acts as a "linguistic butcher," using an algorithm like Byte-Pair Encoding (BPE) to slice the novel string into recognizable sub-word chunks from its existing vocabulary. This ensures the model practically never encounters an "Unknown Token" error.

## Why Tokenization Matters
Understanding this translation layer is critical for developers for three main reasons:
1.  **Efficiency:** Sub-word tokenization compresses data. A long word might be 12 characters but only 2 or 3 tokens.
2.  **Cost & Limits:** API costs and context windows are measured in tokens, not characters. A general rule of thumb for English is that 1,000 tokens are approximately equal to 750 words.
3.  **Blind Spots:** Because the model only sees integer IDs and not individual letters, it struggles with character-level tasks (like counting the letters in a word). It must rely on statistical probability rather than literal counting.

# Meaning in Math: Vector Embeddings
Integers outputted by the tokenizer carry no semantic meaning on their own. To process language and quantify "meaning," an LLM maps every single token ID in its vocabulary to a **Vector**—a list of floating-point numbers.

## From Integers to Dimensions
Imagine a 3-dimensional coordinate system where axes represent concepts like "animal-like," "domesticated," or "fluffy". In this space, the token for "Dog" and "Wolf" would be mathematically close to each other, while "Rock" would be far away. 

In a real Large Language Model, tokens are represented as vectors in a high-dimensional space, often utilizing thousands of dimensions (e.g., 4,096). 
* Let a token be represented by a vector $\vec{v} \in \mathbb{R}^d$, where $d$ is the number of dimensions. 
* These dimensions are not explicitly labeled by humans; the model learns these latent features entirely on its own during its initial training phase by observing which words frequently appear together.

## Semantic Arithmetic
Because meaning is mapped to numerical coordinates, the model can perform linear algebra to manipulate concepts. This transforms human semantics into pure spatial geometry.
$$\vec{v}_{\text{King}} - \vec{v}_{\text{Man}} + \vec{v}_{\text{Woman}} \approx \vec{v}_{\text{Queen}}$$

## Measuring Closeness: Cosine Similarity
While a developer might intuitively use Euclidean Distance (the magnitude or absolute distance between two points) to measure similarity, this metric is susceptible to frequency bias. A common word might have a very long vector, causing it to appear distant from a rare synonym simply because of its frequency in the training data.

Instead, LLMs rely on **Cosine Similarity** to measure the *direction* the vectors are pointing.
* By measuring the angle ($\theta$) between two vectors, we isolate their pure semantic direction.
* **$0^\circ$ (Cosine 1.0):** Pointing in the exact same direction (Maximum similarity).
* **$90^\circ$ (Cosine 0.0):** Perpendicular vectors (No similarity).
* **$180^\circ$ (Cosine -1.0):** Pointing in opposite directions (Exact opposite meaning).

Mathematically, it is calculated using the dot product of the vectors divided by the product of their magnitudes:
$$\text{Cosine Similarity} = \frac{\vec{A} \cdot \vec{B}}{\|\vec{A}\| \|\vec{B}\|}$$

## The Embedding Matrix
The vectors are stored in a massive lookup table called the **Embedding Matrix**. 
* When the tokenizer outputs a token ID (e.g., `104`), the model goes to row `104` in this matrix and extracts the entire row of floating-point numbers.
* Mathematically, this is represented as $W_E \in \mathbb{R}^{V \times d}$, where $V$ is the Vocabulary size and $d$ is the dimensionality.

## The Order of Operations (Architecture Constraints)
Understanding the chronological sequence of building an LLM is critical for preventing architectural errors:

1. **Building the Tokenizer (The Dictionary):** An algorithm scans a massive corpus and assigns sequential integer IDs to text chunks (e.g., `0` to `99,999`). This vocabulary is then **locked**.
2. **Initializing the Embedding Matrix (The Blank Canvas):** The model allocates memory for a matrix where the rows exactly match the Tokenizer's vocabulary size. Every row is initially filled with completely random numbers.
3. **Training the Model (Learning the Math):** Through backpropagation, the model constantly adjusts the numbers in the matrix to learn semantic meanings based on the training data.

**The "New Word" Bottleneck:** Because the Tokenizer's IDs map strictly to the matrix's rows, you cannot simply append a new slang word to the Tokenizer after training. Doing so would cause a fatal "Index Out of Bounds" error when the model tries to access a row that does not exist in the $V \times d$ matrix. If engineers surgically append a new row, it starts with random numbers and requires costly fine-tuning to integrate into the learned semantic geometry.

# The Engine: Self-Attention & The Transformer
Up to this point in the architecture, a major flaw exists: in the static Embedding Matrix, a word only has *one* vector. Because language is heavily dependent on context, these static embeddings are inherently context-blind. 

For example, the word "Bank" has a single row of numbers in the lookup table. If a user prompts the model with "deposit money at the bank" versus "pitched my tent on the river bank," a static model would use the exact same vector for both and become hopelessly confused.

## Self-Attention
The **Transformer architecture** (the "T" in GPT) solves this via its core mechanism: **Self-Attention**. 
* **Definition:** Self-attention is an algorithm that allows every single token in a sequence to "look" at every other token in that same sequence. 
* It calculates a mathematical "Attention Score" to determine how strongly each word should influence the meaning of the others.

## How Context Alters the Vector
Instead of relying solely on the static dictionary definition, the Transformer actively updates the vectors based on their surroundings.

1.  **The Query:** When the sequence "river bank" enters the Transformer, the token for "bank" projects a mathematical query, essentially asking the surrounding tokens: *"Who am I related to?"*
2.  **The Score:** The token for "river" responds, and the mechanism calculates a high attention score between the two words.
3.  **The Shift:** The model takes the static, original vector for "bank" and adds a fraction of the "river" vector to it. 

The vector physically moves in the high-dimensional space. It is no longer a generic "bank" vector; it has become a highly specific, dynamically updated *contextual vector* pointing toward the concept of nature and water.

**Example:**
Consider the sentence: *"The alien spaceship landed on the earth, displacing a massive amount of earth."*
While both instances of the word "earth" begin their journey by pulling the exact same static vector from the Embedding Matrix, they end their journey at the end of the Transformer network as completely different mathematical entities. The Self-Attention mechanism blends the first "earth" with "alien" and "planet," while the second "earth" is mathematically blended with "displacing" and "material".

# The Workspace: Context Window
Every interaction with an LLM occurs within a strict boundary known as the **Context Window**. 

If the Embedding Matrix serves as the model's long-term memory (the semantic geometry it learned during training), the Context Window acts as its **short-term memory or RAM**. It defines the maximum number of tokens the model can hold in its "mind" at any single exact moment during inference (when generating a response).
* Early LLMs possessed tiny context windows of around 512 tokens (roughly one page of text).
* Modern standard models operate with 8,000 to 128,000 tokens, while cutting-edge models can reach up to 2,000,000 tokens.

## The Quadratic Bottleneck
The hard limit on context window size is dictated by the mathematics of the Self-Attention mechanism. 

Because Self-Attention requires *every* token to calculate an attention score with *every other* token in the sequence to understand the full context, the computational cost explodes as the input grows.
* A 10-token prompt requires a $10 \times 10$ grid (100 calculations).
* A 100-token prompt requires a $100 \times 100$ grid (10,000 calculations).

In computer science, this scaling behavior is denoted using Big O notation as **$O(N^2)$**, where $N$ is the number of tokens. The mechanism scales **quadratically**, meaning the Video RAM (VRAM) required on the GPUs to store these attention matrices increases exponentially.

## Mathematical Scaling Example
Because of this quadratic constraint, expanding a context window is an enormous engineering challenge. 
If a baseline document of **1,000 tokens** consumes **2 GB of VRAM**, increasing the input size requires squaring the multiplier to find the final memory footprint:

* **Double the input ($2x$):** The squared multiplier is $2^2 = 4$. Total VRAM: $4 \times 2\text{ GB} = \textbf{8 GB}$.
* **Quadruple the input ($4x$):** The squared multiplier is $4^2 = 16$. Total VRAM: $16 \times 2\text{ GB} = \textbf{32 GB}$.
* **Quintuple the input ($5x$):** The squared multiplier is $5^2 = 25$. Total VRAM: $25 \times 2\text{ GB} = \textbf{50 GB}$.

This geometric explosion explains why processing massive documents via an LLM API costs significantly more compute and money than a short paragraph.

# The Controls: Inference Parameters
When an LLM processes a prompt, it does not simply calculate the single "correct" next word. Instead, the final layer of the Transformer outputs a **Probability Distribution** across the *entire* vocabulary (e.g., all 100,000 tokens). 

If the model consistently picked the #1 most statistically likely token (a strategy known as "Greedy Decoding"), its output would be highly repetitive and robotic. To make the model useful for diverse use cases, developers manipulate this probability distribution using specific inference parameters before the model selects the next token.

## 1. Temperature
Temperature acts as a scaling factor applied to the final calculations before percentages are determined. It is effectively the **"Creativity vs. Determinism" dial**.
* **Low Temperature (e.g., 0.0 to 0.2):** This artificially inflates the highest probabilities and suppresses the lower ones. The model becomes highly predictable, factual, and deterministic. This is ideal for strict use cases like code generation or database querying, where rigid syntax is required.
* **High Temperature (e.g., 0.8 to 0.9):** This flattens the probability distribution. The model is more likely to select "sub-optimal" or less probable words, leading to more varied, creative, and sometimes hallucinatory text. This is better suited for creative writing or brainstorming.

## 2. Top-K Sampling
This parameter enforces a hard, numerical cutoff on the available vocabulary. 
* If set to `Top-K = 40`, the model ranks all tokens by probability and keeps only the top 40 options. 
* It completely deletes the remaining tens of thousands of options and recalculates the percentages among the surviving 40. 
* This prevents the model from ever selecting a completely nonsensical word, even when operating at high temperatures.

## 3. Top-P (Nucleus Sampling)
Instead of restricting the pool to a fixed *number* of tokens, Top-P looks at the *cumulative probability*. 
* If set to `Top-P = 0.90`, the model goes down the ranked list of tokens and adds their probabilities together. 
* Once the running total hits 90%, it discards all remaining tokens. 
* Unlike Top-K, Top-P dynamically shrinks or expands the pool of choices based on how confident the model is in its top predictions.