# Nokia RAG Pipeline Assignment 

## Project Overview
This project implements a Retrieval-Augmented Generation (RAG) system inside a Google Colab notebook to query technical documentation regarding Nokia 1830 PSS equipment. The notebook covers data ingestion, text chunking, vector retrieval, and robust out-of-domain query handling.

---

## How to Run the Notebook
1. Open the provided `.ipynb` file directly in your browser using **Google Colab**, or upload it to your Colab workspace.
2. Make sure to upload the required PDF data files/documents to your Colab environment if prompted by the file paths in the code.
3. Add your API keys (e.g., OpenAI API Key) in the designated cell or via Colab's **Secrets** tab (key named `OPENAI_API_KEY`).
4. Run all cells sequentially (**Runtime -> Run all**) to execute the pipeline, set up the vector store, and test the queries.

---

## Key Features & Implementation Details
* **Data Parsing & Chunking**: Text and tables are split with a maximum chunk size of 2000 characters, utilizing a split-by-title and recursive approach to preserve structural headings and equipment context.
* **Retrieval System ($k$ and Hybrid Search)**: Configured to retrieve the top $k = 5$ most relevant context chunks. A **Hybrid Search** approach combining dense vector embeddings (ChromaDB + Sentence Transformers `all-MiniLM-L6-v2`) with sparse keyword matching (`BM25Okapi`) was implemented to fix retrieval misses on specific technical part numbers.
* **Handling "I Don't Know" (Q8 Guardrails)**: Implements system instructions and prompt constraints so the model refuses to hallucinate and correctly states when information is missing, following the rule: *"A RAG system that never says 'I don't know' is more dangerous than no RAG system at all."*

---

## System Prompt Used
```text
# Persona
You are an expert Nokia optical transport systems engineer with 10 years of experience.

# Task
Answer the user's question strictly using ONLY the direct facts found in the provided context chunks. If the core information is missing, state clearly that the documents do not contain it.

# Context
Context chunks retrieved from Nokia technical documentation.

# Format
Provide your response in a concise, structured bulleted list (keep it under 150 words). Do not include introductory phrases.

# Tone
Professional, objective, precise, and straight to the point.

# Guidelines
1. Keep your answer extremely direct, concise, and straight to the point.
2. Answer ONLY what is asked in the question. Do NOT include extra details or conversational fillers.
3. If the core information is missing from the chunks, state that the documents do not contain it.
4. If the question has multiple parts, you are fully allowed and encouraged to connect information from different chunks to provide a complete answer.
```
## Evaluation Table (All 8 Questions)
<img width="1244" height="405" alt="Screenshot (1226)" src="https://github.com/user-attachments/assets/1fcc3fd0-72f3-4ede-a1d9-b883c69e5afa" />
