# Domain-Constrained Chatbot Using Fine-Tuned GPT-3.5 and R Shiny

This repository contains an end-to-end implementation of a **domain-specific chatbot** built using a **fine-tuned OpenAI GPT-3.5 model**, with all data preparation, inference, and deployment handled in **R**. The chatbot is deployed through a **Shiny web application** and is designed to answer questions strictly within a predefined knowledge scope.

---

## Project Summary

The goal of this project is to demonstrate how to:

- Prepare structured training data for OpenAI fine-tuning
- Fine-tune a chat-based language model for a closed domain
- Query the fine-tuned model via the OpenAI Chat Completions API
- Deploy the model in an interactive Shiny chatbot interface
- Constrain model behavior using system prompts and static context

This project focuses on **controlled question–answering**, not open-ended generation.

---

## Key Characteristics

- **Language**: R  
- **Model**: Fine-tuned `gpt-3.5-turbo`  
- **Frontend**: Shiny  
- **Grounding Strategy**:
  - Fine-tuning on curated Q&A data
  - Prompt-based context injection (static knowledge)

> ⚠️ This project does **not** implement a true Retrieval-Augmented Generation (RAG) pipeline.  
> There is no vector database, embedding generation, or dynamic retrieval at inference time.

## Repository Structure

├── Training_Data.csv # Question–answer pairs
├── Training_Data.jsonl # Converted JSONL for fine-tuning
├── data_preparation.R # CSV → JSONL conversion
├── inference_test.R # Direct API call to fine-tuned model
├── shiny_app.R # Shiny chatbot application
└── README.md


---

## 1. Training Data Preparation

Training data is stored as a CSV file containing `question` and `answer` columns.  
This data is converted into OpenAI’s **JSONL chat format** using R.

Each training example follows this structure:

```json
{
  "messages": [
    { "role": "system", "content": "You are a teaching assistant for Machine Learning." },
    { "role": "user", "content": "<question>" },
    { "role": "assistant", "content": "<answer>" }
  ]
}
The conversion process ensures compatibility with OpenAI’s fine-tuning API.

2. Fine-Tuned Model Inference

Once fine-tuned, the model is accessed via the OpenAI Chat Completions API.

Key configuration choices:

Low temperature for deterministic responses

Controlled token limits

No external knowledge injection beyond training data or prompts

This ensures consistent, domain-aligned outputs.

3. Prompt-Based Grounding (Static Context)

In addition to fine-tuning, the project demonstrates context injection via system prompts.

A complete story or knowledge base (e.g., a fictional character biography) is passed as a system message at inference time. The model is instructed to answer only using this provided content.

This approach:

Reduces hallucinations

Works well for small, static domains

Does not scale to large or frequently updated knowledge bases

4. Shiny Chatbot Application

The Shiny app provides:

A text input for user questions

Real-time responses from the fine-tuned model

A visible conversation log

The application communicates directly with the OpenAI API using httr and parses responses using jsonlite.

This setup is suitable for:

Prototyping

Demonstrations

Internal tools

Educational use cases

What This Project Is

A fine-tuned question–answering system

A demonstration of model grounding via training and prompts

A practical example of LLM integration in R

A lightweight chatbot deployment using Shiny

What This Project Is Not

A Retrieval-Augmented Generation (RAG) system

A vector-search-based chatbot

A production-scale knowledge assistant

A dynamic document retrieval system

When to Use This Architecture

This approach is well suited for:

Fictional worlds and narratives

Fixed documentation

Educational assistants

Simulations and experiments

Small, closed knowledge domains

For large or frequently changing datasets, a full RAG pipeline with embeddings and retrieval should be used instead.

Technologies Used

R

Shiny

OpenAI Chat Completions API

Fine-tuned GPT-3.5

jsonlite

httr

## Repository Structure

# Fine-Tuned-Domain-Specific-Chatbot-with-R-and-Shiny
