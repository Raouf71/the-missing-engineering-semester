## 🧠 GenAI / RAG – Self-Check Evaluation Set:

<ins> Check 1 - Problem Understanding Check:</ins>

> “Design a RAG system for answering internal company questions.”

- ❌ Weak answer
“I’d use an LLM, embed the documents, store them in a vector database, and retrieve relevant chunks.”

- ✅ Strong answer
“Before designing, I’d clarify the goal: who the users are, what accuracy they expect, and whether latency or cost is more critical.”

        ♟️ Strategy/Methodology:
            - Clarify goal, users, constraints
            - Don’t jump to tools
            -> Terms: use case, user intent, constraints

<ins> Check 2 - Assumptions Check:</ins>

> “How would you improve answer quality in a RAG system?”

- ❌ Weak answer
“I’d increase the chunk size and add more documents.”

- ✅ Strong answer
“Assuming documents are high-quality and mostly text-based, I’d first evaluate retrieval quality before modifying chunking or model size.”

        - ♟️ Strategy/Methodology:
            - Explicit assumptions
            - Awareness of data quality
            -> Terms: assumption, retrieval quality, data distribution

<ins> Check 3 - Solution Framing Check:</ins>

> “Explain your RAG architecture.”

- ❌ Weak answer
“We used FAISS, OpenAI embeddings, and GPT-4 with a prompt template.”

- ✅ Strong answer
“At a high level, the system has three stages: ingestion, retrieval, and generation, each with clear responsibilities.”

        ♟️ Strategy/Methodology:
            - High-level structure first
            - Components before tools
            -> Terms: pipeline, modularity, responsibilities

<ins> Check 4 - Trade-Off Check:</ins>

> “Would you use a larger or smaller LLM for RAG?”

- ❌ Weak answer
“A larger model because it’s more accurate.”

- ✅ Strong answer
“A larger model improves reasoning, but increases latency and cost. For high query volume, I’d prefer a smaller model with strong retrieval.”

        ♟️ Strategy/Methodology:
            - Explicit trade-offs
            - Business + system thinking
            -> Terms: latency, cost, accuracy trade-off

<ins> Check 5 - Constraints Alignment Check:</ins>

> “How would you deploy a RAG system for real-time customer support?”

- ❌ Weak answer
“I’d use the best model available to ensure high-quality answers.”

- ✅ Strong answer
“Given real-time constraints, I’d prioritize low-latency retrieval, limit context size, and cache frequent queries.”

        ♟️ Strategy/Methodology:
        - Real-world constraints
        - Practical design choices
        -> Terms: real-time, latency budget, caching

<ins> Check 6 - Risk & Failure Check:</ins>

> “What can go wrong in a RAG system?”

- ❌ Weak answer
“Sometimes the model hallucinates.”

- ✅ Strong answer
“Major risks include retrieval failure, outdated documents, and prompt leakage. I’d mitigate this with confidence scoring and source grounding.”

        ♟️ Strategy/Methodology:
            - Identifies specific failure modes
            - Mentions mitigation
            -> Terms: hallucination, grounding, failure modes

<ins> Check 7 - Validation & Testing Check:</ins>

> “How would you evaluate your RAG system?”

- ❌ Weak answer
“I’d check if answers look good.”

- ✅ Strong answer
“I’d evaluate retrieval precision, answer faithfulness, and latency, using both offline benchmarks and user feedback.”

        ♟️ Strategy/Methodology:
            - Measurable evaluation
            - Multi-layer testing
            -> Terms: precision@k, faithfulness, metrics

<ins> Check 8 - Communication Check:</ins>

> “Explain RAG to a product manager.”

- ❌ Weak answer
“We embed documents and do semantic search with vectors.”

- ✅ Strong answer
“RAG lets the model answer questions using company data instead of guessing, which improves accuracy and trust.”

        ♟️ Strategy/Methodology:
            - Audience-aware explanation
            - Clear value framing
            -> Terms: trust, accuracy, business impact

<ins> Check 9 - Ego & Honesty Check:</ins>

> “Why did your RAG system fail in production?”

- ❌ Weak answer
“The data wasn’t good and the model had limitations.”

- ✅ Strong answer
“I underestimated document drift. In hindsight, I should’ve added monitoring and re-indexing earlier.”

        ♟️ Strategy/Methodology:
            - Ownership
            - Learning mindset
            -> Terms: monitoring, hindsight, iteration

<ins> Check 10 - End-Answer Close Check:</ins>

> “Is your RAG design final?”

- ❌ Weak answer
“Yes, this is the best architecture.”

- ✅ Strong answer
“It works under current constraints, but I’d revisit model size or retrieval strategy if scale or latency requirements change.”

        ♟️ Strategy/Methodology:
            - Flexibility
            - Context awareness
            -> Terms: constraints, adaptability, iteration