# PerfumeRecommender

An intelligent, explainable perfume recommendation system that combines **Retrieval-Augmented Generation (RAG)** with a **multi-LLM ensemble** and a unique **judging mechasnim** to deliver personalised scent suggestions based on natural language queries

This project leverages a curated perfume dataset, ChromaDB for vector search, and leading large language models (LLaMA 3, Mistral, Phi-3) to match user preferences with real perfumes, offering rich justifications for each recommendation.

## Features
- **Semantic Search (RAG)**: Retrieves the most relevant perfumes using sentence embeddings and ChromaDB
- **LLM Ensemble**: Routes the same prompt through multiple hosted LLMs for diverse reasoning
- **Grounded Generation**: Ensures recommendations are always drawn from the real, embedded perfume database
- **Judging/Fusion Logic**: Combines or ranks outputs from different LLMs into a single coherent response

## How It Works
1. **Input**: User submits a natural language query (e.g “I want a floral unisex perfume for spring mornings”)
2. **Semantic Retrieval**: Top-k matching perfumes are retrieved from a vector database (ChromaDB) using sentence-transformer embeddings
3. **Prompt Construction**: The retrieved perfumes are passed as context into a custom prompt for the LLMs
4. **LLM Response**: The prompt is sent to multiple LLMs (LLaMA 3, Mistral, Phi-3), each returning their top 2–3 recommendations
5. **Judging LLM**: A judging LLM fuses, deduplicates, and explains the final result
6. **Output**: The system returns a list of 2–3 well-justified perfume recommendations tailored to the user's request

## Why Use an LLM Blender?
### 1. LLM‑Blender: PairRanker + GenFuser: https://arxiv.org/abs/2306.02561
LLM‑Blender (Jiang et al., 2023) is a two-stage ensemble framework:

PairRanker: performs pairwise comparisons between candidate outputs to select the best ones, capturing subtle qualitative differences.

GenFuser: fuses the top-ranked outputs via a seq2seq model to produce a stronger final output.

On the MixInstruct benchmark, LLM‑Blender significantly outperforms individual models and other ensemble baselines, achieving better scores in metrics like BERTScore, BARTScore, BLEURT, and GPT‑Rank correlations. It ranks in the top‑3 in 68.6% of cases, compared to Vicuna’s ~52.9% (Jiang et al., 2023).

Why it matters:

Different open‑source LLMs excel on different inputs—no single model dominates across all queries (Vicuna only ranks first ~21%). A dynamic ensemble helps match strengths to specific queries (Jiang et al., 2023).

Ranking and fusing avoids noise from low-quality candidates, improving robustness and output quality beyond naïve voting or average scoring (Jiang et al., 2023).

### 2. Broader Ensemble & Blending Research
https://arxiv.org/abs/2401.02994 "Blending Is All You Need" (Lu et al., 2024): blending smaller models (6B/13B) can match or exceed performance of much larger models like ChatGPT (175B), leveraging their complementary strengths.


## Unique Judging Mechanism: LLM-as-a-Judge for Output Evaluation
Unlike traditional LLM ensemble approaches like LLM‑Blender, which rely on ranking (PairRanker) and fusion (GenFuser), this system introduces a distinct and novel component: a Judging mechanism.

This Judging module functions as an LLM-as-a-Judge, evaluating the outputs of multiple LLMs based on semantic quality, relevance, and explanatory clarity. It provides a scored assessment of each candidate response—ensuring that only high‑quality recommendations make it to the final output.

### Why This Matters
Not present in LLM-Blender: PairRanker performs pairwise comparison, but does not use a dedicated LLM to score and judge candidate responses against a rubric or human-aligned criteria.

Inspired by emerging research on LLM judges and multi-agent evaluation pipelines (Li et al., 2025, Weng et al., 2024), this component adds an extra layer of qualitative filtering and control.

Enables domain-specific evaluation: In our case, the judge is tuned to value creativity, scent profile accuracy, and explanatory coherence for perfume recommendations.

### Judging Workflow
Multiple LLMs generate responses based on the retrieved perfume context.

A Judge LLM scores these responses using a custom rubric prompt (e.g., “Rate this recommendation for clarity, creativity, and fit”).

The highest-rated output is either selected directly or passed into a fusion step for refinement.

This layered approach ensures that our final recommendation is a blend and a reasoned, evaluated, and curated answer, offering more trustworthiness and alignment with user needs.

