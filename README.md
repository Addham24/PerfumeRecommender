# PerfumeRecommender

An intelligent, explainable perfume recommendation system that combines **Retrieval-Augmented Generation (RAG)** with a **multi-LLM ensemble** to deliver personalised scent suggestions based on natural language queries

This project leverages a curated perfume dataset, ChromaDB for vector search, and leading large language models (LLaMA 3, Mistral, Phi-3) to match user preferences with real perfumes, offering rich justifications for each recommendation.

## Features
- **Semantic Search (RAG)**: Retrieves the most relevant perfumes using sentence embeddings and ChromaDB
- **LLM Ensemble**: Routes the same prompt through multiple hosted LLMs for diverse reasoning
- **Grounded Generation**: Ensures recommendations are always drawn from the real, embedded perfume database
- **Judging/Fusion Logic**: Combines or ranks outputs from different LLMs into a single coherent response

## How It Works
1. **Input**: User submits a natural language query (e.g “I want a floral unisex perfume for spring mornings”)
2. **Brand Extraction**: System parses the brand from the query (if any)
3. **Semantic Retrieval**: Top-k matching perfumes are retrieved from a vector database (ChromaDB) using sentence-transformer embeddings
4. **Prompt Construction**: The retrieved perfumes are passed as context into a custom prompt for the LLMs
5. **LLM Response**: The prompt is sent to multiple LLMs (LLaMA 3, Mistral, Phi-3), each returning their top 2–3 recommendations
6. **Judging LLM)**: A judging LLM fuses, deduplicates, and explains the final result
7. **Output**: The system returns a list of 2–3 well-justified perfume recommendations tailored to the user's request
