# Structured Information Extraction
---
## Project Overview
Clinical trial abstracts contain large amounts of unstructured biomedical text, making it difficult for researchers to efficiently extract and synthesise key evidence across studies. Converting this information into structured formats enables easier comparison, retrieval, and downstream analysis in evidence-based medicine.

This project addresses the task of **structured information extraction** from clinical trial abstracts. The objective is to transform unstructured text into a predefined tabular schema and evaluate how different extraction strategies influence the quality of the extracted information.

The dataset used in this work is the **EBM-NLP corpus** (https://github.com/bepnye/EBM-NLP), which consists of approximately 5,000 clinical trial abstracts annotated for evidence-based medicine tasks. The dataset provides token-level annotations for key schema elements: **Participants (PART)**, **Intervention (INT)**, and **Outcome (OUT)**. 

In addition to extraction, this project also explores whether **unsupervised clustering methods like K-Means** can reveal natural groupings that align with schema components before performing structured extraction.

---

## Objectives

The main objectives of this project are:

- To extract structured information from clinical trial abstracts into a predefined schema consisting of:
  - Participants (PART)
  - Intervention (INT)
  - Outcome (OUT)

- To design and implement multiple information extraction pipelines based on different methodological strategies, including:
  - Rule-based approaches
  - Statistical models (e.g., Conditional Random Fields)
  - Large Language Models (zero-shot and few-shot prompting)
  - Fine-tuned transformer models using LoRA

- To explore different design dimensions such as:
  - Rule-based vs LLM-based approaches
  - Zero-shot, few-shot, and supervised learning strategies

- To investigate whether unsupervised clustering methods can reveal latent structure in clinical trial sentences prior to extraction

- To evaluate and compare all approaches using:
  - Field-level performance (Precision, Recall, F1-score)
  - Coverage vs precision trade-offs
  - Practical usability for downstream biomedical information retrieval tasks

- To analyse the strengths, limitations, and trade-offs between different extraction strategies

## Model-Level Performance (Micro-Averaged)

| Model                         | Precision | Recall | F1-score |
|------------------------------|----------|--------|----------|
| Rule-based (Bigram) | 0.07     | 0.04   | 0.05     |
| spaCy Rule-based              | 0.14     | 0.18   | 0.15     |
| CRF                           | 0.41     | 0.21   | 0.28     |
| LLM Zero-shot                | 0.79     | 0.40   | 0.46     |
| LLM Few-shot                | 0.84     | 0.55   | 0.61     |
| LoRA Fine-tuned LLM         | 0.85     | 0.59   | 0.64     |

## Macro-Averaged Performance (Class-balanced Evaluation)

| Model                         | Macro Precision | Macro Recall | Macro F1 |
|------------------------------|----------------|--------------|----------|
| Rule-based (Unigram + Bigram) | 0.08           | 0.04         | 0.05     |
| spaCy Rule-based              | 0.12           | 0.18         | 0.13     |
| CRF                           | 0.38           | 0.20         | 0.26     |
| LLM Zero-shot                | 0.62           | 0.40         | 0.46     |
| LLM Few-shot                | 0.70           | 0.55         | 0.61     |
| LoRA Fine-tuned LLM         | 0.73           | 0.59         | 0.64     |

## Field-Level Performance (F1-score by Entity Type)

| Model                         | INT F1 | OUT F1 | PART F1 |
|------------------------------|--------|--------|----------|
| Rule-based (Unigram + Bigram) | 0.38   | 0.33   | 0.22     |
| spaCy Rule-based              | 0.25   | 0.04   | 0.11     |
| CRF                           | 0.36   | 0.25   | 0.16     |
| LLM Zero-shot                | 0.38   | 0.33   | 0.22     |
| LLM Few-shot                | 0.50   | 0.58   | 0.43     |
| LoRA Fine-tuned LLM         | 0.55   | 0.61   | 0.47     |
