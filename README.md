![logo_ironhack_blue 7](https://user-images.githubusercontent.com/23629340/40541063-a07a0a8a-601a-11e8-91b5-2f13e4e6b441.png)

# Lab | Build the RAG Pipeline

## Overview

Yesterday you retrieved passages by hand. Today you turn those passages into **answers**. You'll load the same knowledge base into a real vector store, then build the full **retrieve → augment → generate** loop: a question goes in, the right passages come out, and a language model writes a grounded, **cited** answer using them.

The two ideas that make a RAG system trustworthy are the focus: every answer must **cite its source**, and the system must be willing to say **"I don't know"** when the knowledge base doesn't contain the answer.

This is lab 2 of the connected RAG track — same `knowledge_base.json` as yesterday.

## Learning Goals

- Index a corpus into a vector store (Chroma) instead of searching by hand
- Assemble retrieved passages into a grounded prompt with citations
- Make the system refuse to answer when the context doesn't support it

## Setup

Fork, clone, branch. Reuse your Gemini key from Unit 8.

```bash
pip install -r requirements.txt
export GOOGLE_API_KEY="your-free-gemini-key"
```

You'll use **Chroma** as the vector store and a Gemini chat model for generation. Work in `rag_pipeline.ipynb` or `.py`.

## Your Task

**Build a working RAG pipeline over the knowledge base and answer questions with citations.**

1. **Index:** load `knowledge_base.json`, embed each passage, and add it to a Chroma collection together with its `source` as metadata. (Each entry is already a sensible chunk, so no splitting is needed today — note in a comment *why* you'd need to chunk if these were full documents.)
2. **Query:** write a function that takes a question, retrieves the **top 3** passages from Chroma, and assembles a prompt containing: an instruction to answer **only** from the context and to **say "I don't know" if it's absent**, the retrieved passages each tagged with its source, and the question.
3. **Generate:** send that prompt to the model and return the answer **with citations** (which source each fact came from).
4. Run these questions and capture the answers:
   - `"How long do I have to get a full refund?"` (answerable)
   - `"How do I reset my password?"` (answerable)
   - `"What is the company's stock price today?"` (**not** in the knowledge base — the system must decline)

Your deliverable should show, for each question, the retrieved sources and the final cited answer.

### Optional stretch

For one answerable question, deliberately retrieve only the **top 1** passage instead of top 3 and see whether the answer suffers. Write a sentence on the trade-off between too little and too much context.

## Submission

Commit your code and a short results file (or notebook output) showing the three questions, their retrieved sources, and the cited answers — including the "I don't know" case. Open a PR and paste the link.

## Quality Bar

- Answers are grounded in retrieved passages and **cite their source**
- The out-of-scope question is **declined**, not answered with an invention
- Indexing and querying are clearly separated in the code
- No API key is committed
