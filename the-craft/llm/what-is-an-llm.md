# WIP

## What is an LLM:
* Next token generator
* LLMs are probabilistic generators
* *A high-dimensional statistical engine. The challenge isn't the "Large" part; it's managing hallucination, context window limits, and cost-to-latency trade-offs.
* params: context_window, temperature, etc.

## What is a hallucination
* The model filling gaps, guessing, and producing smooth but incorrect answers
* Hallucination is a system failure, not a model bug

- Solution to reduce it:
    * RAG, but:
        > RAG can reduce hallucinations by grounding responses in data, but it does not guarantee correctness if retrieval fails or context is incomplete

        > RAG reduces hallucination probability — it does not guarantee truth.
        
        > RAG reduces hallucinations by grounding the model in retrieved context instead of relying only on its internal knowledge.