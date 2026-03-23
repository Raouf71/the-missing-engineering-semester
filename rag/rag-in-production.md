## RAG in production

<details>
<summary> <ins> LEVEL 1 — Do you actually understand RAG?</ins> </summary>

> What is a RAG-based system — and what is it not?

- ✅ Strong answer:

    - A RAG-based system is a **pipeline** that combines **retrieval** and **generation**, where an LLM answers queries using **externally** retrieved documents instead of relying only on its internal knowledge.
    - **It does not change the model’s weights like training or fine-tuning**, and it does not make the model inherently smarter — it only conditions generation on retrieved context.
    <!-- - RAG can reduce hallucinations by grounding responses in data, but it does not guarantee correctness if retrieval fails or context is incomplete -->

- Example:
    * **raw LLM**: junior joining company on day 1
    * **raw LLM + Knowledge base**: an engineer with a couple of years of experience
</details>

<details>
<summary> <ins> LEVEL 2 — Why RAG exists (motivation under constraints)?</ins> </summary>

> Q2.1 — Why RAG?

- ✅ Strong answer:
    - Reduce hallucination
    - Up-to date info
    - Handle large datasets
    - Workls with structured as well as unstructured data


> Q2.2 — Why do we need RAG instead of just using a bigger or better LLM?

- ✅ Strong answer:
    - Using a bigger or better LLM usually means fine-tuning or retraining, which is:
        - expensive
        - slow
        - Fine-tuning doesn’t guarantee coverage of private or frequently changing data.
    - **LLMs are strong at reasoning and language generation, but weak at staying up-to-date and reliably recalling specific facts**.
        - RAG **separates** knowledge from reasoning by retrieving relevant documents at query time, which is 
            - cheaper
            - more scalable
            - easier to update
        - This also allows us to **control cost, latency, and data access** without modifying the model itself

- 🧠 Mental anchor:
    > LLMs reason. Systems supply knowledge.

</details>

<details>
<summary> <ins> LEVEL 3 — Retrieval is the real system (not the LLM)</ins> </summary>

> Most RAG failures are retrieval failures. Explain why.

- ✅ Strong answer:
    - Generation quality is bounded by the **relevance and completeness of retrieved context.**
        - incomplete, misleading, or irrelevant documents &rarr; LLM has **no reliable** information to reason over &rarr; produce confident but wrong answers.
    - Retrieval can fail due to:
        - poor data ingestion
        - suboptimal chunking
        - weak retrieval methods that don’t match data structure
        - ambiguous user queries

- 🧠 Mental anchor:
    > In RAG, the LLM can only be as good as the context you retrieve.

</details>

<details>
<summary> <ins> LEVEL 4 — Failure modes</ins> </summary>

> Explain how suboptimal chunking breaks a RAG system — even with a strong model.

- ✅ Strong answer:

    - Suboptimal chunking impacts what can be retrieved and therefore what the model can reason over.
        - If chunks are **too small** &rarr; important context like tables or footnotes can be split &rarr; individual chunks incomplete or misleading &rarr; incorrect answers.
        - If chunks are **too large** &rarr; context becomes less precise &rarr; irrelevant information and noise &rarr; incorrect answers.

    ⚠️ It also affects multi-hop queries, where relevant information may be distributed across multiple chunks but not retrieved together.
</details>


<details>
<summary> <ins> LEVEL 5 — Embedding Drift</ins> </summary>

> Q5.1 — What is embedding drift, and why is it dangerous in production RAG systems?

- ✅ Strong answer:
    - Embedding drift refers to changes in the embedding space over time, typically caused by:
        - updating embedding model 
        - changes in the data distribution.

> Q5.2 — Why is it dangerous in production RAG systems?

- ✅ Strong answer:
    - Stored document embeddings and new query embeddings may no longer be comparable
        &rarr; silent degradation of retrieval quality.
    * Relevant documents not being retrieved, even though they exist
        &rarr; downstream generation errors.
    
    ⚠️ The failure is often hard to detect because the system still returns results, just less relevant ones.
    
    - 💡 Solution: In production, this is usually mitigated by:
        - versioning embeddings
        - re-indexing when models change
        - monitoring retrieval performance

- 🧠 Mental anchor:
    > If embeddings are not aligned, retrieval is broken — even if nothing crashes (not ALL failures crash).
</details>
    
<details>
<summary> <ins> LEVEL 6 — Hallucinations & Accuracy</ins> </summary>

> Why does RAG reduce hallucinations but never fully eliminate them?

- ✅ Strong answer:
    <!-- - RAG reduces hallucinations by grounding the model in retrieved context instead of relying only on its internal knowledge. -->

    - **Hallucinations in RAG** are often **the result of both** retrieval failures and the generative nature of the model.
    - System still depends on retrieval quality and context completeness.
        - If retrieval is incomplete, misleading, or fails entirely &rarr; model will generate plausible answers to fill the gaps.
    - Additionally, LLMs are probabilistic and don’t verify truth, so they can misinterpret or ignore provided context.

- 🧠 Mental anchor:
    > RAG reduces hallucination probability — it does not guarantee correctness if retrieval fails or context is incomplete.
</details>

<details>
<summary> <ins> LEVEL 7 — Latency vs Accuracy (production trade-off)</ins> </summary>

> You are asked to deploy RAG for real-time customer support. What trade-offs do you make?

- ✅ Strong answer:
    - For real-time customer support, **the main trade-off is between latency, accuracy, and cost**.
        - Limit retrieval depth by reducing top-k results and controlling context size 
        - Use smaller or optimized models for most queries
        - Optionally route complex queries to larger models.
        - Caching frequent queries 
        - Caching precomputed embeddings
    - Overall:
        - **Prioritize** low latency while maintaining acceptable accuracy, **even** if it slightly reduces answer completeness.

- 🧠 Mental anchor:
    > In production RAG: you don’t maximize accuracy — you optimize latency under acceptable accuracy.
</details>

<details>
<summary> <ins> LEVEL 8 — Evaluation</ins> </summary>

> How do you evaluate a RAG system beyond “answers look good”?

- ✅ Strong answer:
    - Evaluation of a RAG system occurs on two levels: 
        - <ins>Retrieval</ins>: Use **metrics** like ``precision@k`` or ``recall@k`` to measure whether relevant documents are being retrieved.
        - <ins>Generation</ins>: Use **metrics** like ``faithfulness`` and ``correctness``, ensuring answers are grounded in the retrieved context.

    - Other methods: 
        - <ins>Offline evaluation</ins>: Use a labeled Q&A dataset (queries with expected answers)
        - <ins>Evaluation-Frameworks</ins>: Use frameworks like RAGAS.
    - <ins>In production</ins>: Monitor user feedback and failure cases to continuously improve both retrieval and generation

- 🧠 Mental anchor:
    > If you don’t separate retrieval from generation, you can’t debug.

</details>

<details>
<summary> <ins> LEVEL 9 — Complex Queries</ins> </summary>

> Why do complex or multi-hop queries break RAG systems?

- ✅ Strong answer:
    - Even though advanced retrieval techniques improve coverage, multi-hop queries still require **reasoning** and **coordination** across results, <ins>which retrieval alone doesn’t guarantee</ins>.

- Solutions:
    - **Query Decomposition**
        → Break complex query into simpler sub-queries
    - **Reranking + Fusion**
        → Combine results from multiple sources intelligently
    - **Knowledge Graph Traversal**
        → Use structured relationships for multi-step queries
    - **Agentic RAG**
        → LLM plans steps, calls retrieval multiple times

- 🧠 Mental anchor:
    > Complex queries are not retrieval problems — they are reasoning workflows.
</details>

<details>
<summary> <ins> LEVEL 10 — Are you production-ready?</ins> </summary>

> If your RAG system starts giving confidently wrong answers, how do you debug it?

- ✅ Strong answer:
    - Debug it **layer by layer**, starting by **separating retrieval from generation**. 
        1. First inspect:
            - retrieved chunks 
            - context sent to the LLM to see whether the answer was already unsupported before generation. 

        2. Then, <ins>if retrieval is poor</ins>:
            - Trace it back through ingestion, parsing, chunking, embedding quality, query handling, and retrieval strategy. 
        3. Then, <ins>if retrieval looks correct</ins>, inspect generation: 
            - prompt instructions, 
            - grounding constraints, 
            - citation behavior
            - whether the model is following the provided context.

        4. Only after that tune model parameters like temperature. 
    
        5. Finally, reproduce the failure on a fixed test set and add it to evaluation so the issue is monitored going forward.