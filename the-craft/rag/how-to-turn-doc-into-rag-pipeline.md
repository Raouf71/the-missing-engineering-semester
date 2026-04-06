# How to Turn a Document into a RAG Pipeline
### A Practical Guidebook for RAG Engineers

---

## The Core Question
Before writing a single line of code, ask yourself:
> *"What would a user actually ask about this document — and what is the smallest piece of information that answers it?"*

> Everything else follows from that.

---

## Step 1 — Read the Document Like a User

Go through the document manually. Don't think about code yet. Ask:
- What questions would someone ask?
- What would they expect as an answer — a summary, a number, a table row?

> **Example:** A user querying a gear catalog asks *"give me a steel spur gear with module 1.0 and torque above 200 Ncm"* — not *"summarize page 3."* That tells you the pipeline needs structured filtering, not just semantic search.

---

## Step 2 — Identify Page/Section Types

Manually categorize every page type in the document. This becomes your **classification layer** — the first gate in your pipeline.

| Page Type | Example | Action |
|---|---|---|
| Cover / TOC | Page 1–2 | Skip or index as reference |
| Reference / Formulas | Formula page | Raw text node |
| Data / Catalog pages | Gear dimension tables | Full structured extraction |

> **Rule of thumb:** if a page type doesn't answer any user question, skip it. If it might, index it as a raw text node.

---

## Step 3 — Identify the Atomic Unit of Information

What is the **smallest meaningful chunk** a user would query?

- Gear catalog → a single table row (one gear variant)
- Legal document → a single clause
- Product manual → a single step or specification

This atomic unit becomes your **child node** in the retrieval graph.

---

## Step 4 — Identify Shared vs. Unique Metadata

Look at your atomic unit and ask: what information is **repeated across many units** vs. what is **unique per unit**?

- **Shared** (e.g. family, material, module — same for all rows on a page) → **parent node**
- **Unique** (e.g. article number, dimensions — different per row) → **child node**

> This parent-child split is what makes retrieval precise. The parent provides context; the child carries the detail.

---

## Step 5 — Identify Query Patterns

How will users query this document? Each pattern maps to a different retrieval strategy:

| Query Pattern | Example | Retrieval Strategy |
|---|---|---|
| Semantic / descriptive | "lightweight plastic gear" | Vector similarity search |
| Exact lookup | "article number SH2023HF" | Exact metadata filter |
| Range / numeric constraint | "torque > 200 Ncm" | Range metadata filter (`>=`, `<=`) |
| Relational / multi-hop | "all steel gears with module 1.0" | Knowledge graph traversal |

> **Key insight:** not all queries are semantic. If your document has numbers, identifiers, or measurable properties — you need metadata filtering, not just embeddings.

---

## Step 6 — Design Your Schema from Query Patterns

Only now define your data schemas. Fields should map directly to **how users will query**, not just what's in the document.

- Fields users filter by exactly → exact-match metadata fields
- Fields users filter by range → numeric metadata fields
- Fields users describe in natural language → goes into embedded text

> **Anti-pattern:** putting raw numbers like `4.5mm` into embedded text. They add noise to the vector and should live in structured metadata instead.

---

## Step 7 — Identify Your Join Keys

If your document has a **parent-child relationship** across pages or sections, identify what field can reliably link them.

Ask: *"If I extract a child record from page 5, how do I know which parent it belongs to?"*

- Build a deterministic ID from shared fields (e.g. `family + module + material`)
- Normalize those fields **before** building the ID — inconsistent casing or language variants will break the join silently

> **This is the most common failure point in multi-page RAG pipelines.** A join key that works on page 1 must work identically on page 15.

---

## Summary

| Step | Question to answer |
|---|---|
| 1. Read like a user | What will people actually ask? |
| 2. Classify page types | What kind of content is on each page? |
| 3. Find the atomic unit | What is the smallest answerable chunk? |
| 4. Split shared vs. unique | What is parent context vs. child detail? |
| 5. Map query patterns | Semantic, exact, range, or relational? |
| 6. Design schema from queries | What fields do users filter or describe by? |
| 7. Find join keys | How do child records link back to their parent? |
