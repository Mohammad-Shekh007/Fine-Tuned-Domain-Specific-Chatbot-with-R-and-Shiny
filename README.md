# Vector-Based RAG: Cosine, Dot Product, and Euclidean Distance Explained

This repository explains **Vector Retrieval-Augmented Generation (RAG)** similarity metrics in a way beginners can understand.  
We cover **Cosine Similarity, Dot Product, and Euclidean Distance**, step-by-step examples, practical considerations, and modern best practices.

---

## 1. What is Parametric vs Non-Parametric Response?

- **Parametric response**: The model answers using only its **internal knowledge**.  
  Example: Standard LLM answering “What is AI?” without searching any external documents.

- **Non-parametric response**: The model **retrieves external information** before answering.  
  Example: RAG retrieves the most relevant documents from a database, then generates an answer.

Think of it like:
- Parametric → your brain only
- Non-parametric → Google + your brain

---

## 2. How RAG Finds Relevant Documents

RAG turns documents and queries into **vectors (lists of numbers)** using embeddings.  
Then it compares vectors to see which document is closest to the query.  
This is where **similarity metrics** come in.

---

## 3. Cosine Similarity

### What it Measures
- Only the **direction** of vectors.  
- Ignores magnitude (length of vectors).  
- Values range from -1 (opposite) to 1 (same direction).

### How to Calculate
1. Take the query and document vectors: `Q` and `D`.  
2. Compute the dot product: sum of pairwise multiplications.  
3. Divide by the product of their lengths (norms).

\[
\text{Cosine}(Q, D) = \frac{Q \cdot D}{||Q|| \times ||D||}
\]

### Intuition
- If vectors point in the same direction → very similar.  
- If vectors point in opposite directions → very different.  
- Magnitude doesn’t matter: short or long text gives similar score if direction is same.

### Example
Query: `[1, 0, 1]`  
Document: `[2, 0, 2]`  

- Dot product = 1×2 + 0×0 + 1×2 = 4  
- Norms: ||Q|| = √(1+0+1)=1.414, ||D|| = √(4+0+4)=2.828  
- Cosine similarity: 4 / (1.414 * 2.828) ≈ 1 → same direction

---

## 4. Dot Product

### What it Measures
- Combines **direction + magnitude**.  
- High dot product = vectors pointing in the same direction **and/or** vectors are long (large values).

### Example
Query: `[1, 0, 1]`  
Document A: `[1, 0, 1]` → Dot = 2  
Document B: `[2, 0, 2]` → Dot = 4  

- Same direction, but Document B has higher magnitude → higher score.  

### Modern 2026 Best Practice
> Normalize vectors to **unit length** before storing.  
> Then dot product = cosine similarity.  
> This allows **fast dot product computation** while getting the correct directional similarity.

### Intuition
- Dot product without normalization favors long documents.  
- With normalization, dot product behaves like cosine similarity → better for retrieval.

---

## 5. Euclidean Distance

### What it Measures
- Absolute **distance between vectors**.  
- Smaller distance → more similar.

### Step-by-Step
1. Compute element-wise differences: `diff_i = query_i - doc_i`  
2. Square each difference → penalizes large gaps  
3. Sum all squared differences  
4. Take square root → final distance  
5. The **smallest distance wins** → closest document

### Example
Query: `[1, 0, 1]`  
Document: `[2, 0, 2]`  

- Differences: `[1, 0, 1]`  
- Squares: `[1, 0, 1]`  
- Sum: 2  
- Square root: √2 ≈ 1.414 → distance

### Intuition
- Measures “how far apart” vectors are.  
- Sensitive to document length → longer docs may appear further away even if content is relevant.  
- Pooling method (mean, sum, max) matters.  
- Chunking (sentence vs paragraph vs document) also affects results.

---

## 6. Quick Comparison Table

| Metric            | What it measures       | Sensitive to      | Pros                           | Cons                                         |
|------------------|----------------------|-----------------|--------------------------------|----------------------------------------------|
| Cosine Similarity | Direction only        | Vector length    | Robust to long/short documents | Ignores magnitude                            |
| Dot Product       | Direction + magnitude | Vector length & norms | Can be speed-optimized with normalization | Unnormalized: biases longer documents       |
| Euclidean Distance| Absolute distance     | Vector values & length | Intuitive, easy to understand  | Sensitive to document length & pooling       |

---

## 7. Key Takeaways for Beginners

- Cosine → think “are vectors pointing in same direction?”  
- Dot → think “same direction **and** length matters”  
- Euclidean → think “how far apart vectors are in space”  
- Pre-normalize vectors for speed → dot = cosine  
- Chunking, pooling, and metric choice all affect retrieval quality

---

## References & Further Reading

- [RAG: Retrieval-Augmented Generation (Lewis et al., 2020)](https://arxiv.org/abs/2005.11401)  
- [Dense Passage Retrieval (DPR)](https://arxiv.org/abs/2004.04906)  
- [FAISS: Efficient Similarity Search](https://github.com/facebookresearch/faiss)  
- [Understanding Word Embeddings](https://towardsdatascience.com/understanding-word-embeddings-443b7c5b0a7f)

---

> This repo is a **conceptual guide** for beginners and intermediate learners.  
> No code is included—focus is on **understanding the math, intuition, and modern engineering tricks** behind vector similarity in RAG systems.
