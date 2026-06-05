# Structured Information Extraction
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

## Dataset

This project uses the **EBM-NLP dataset**, a large-scale corpus developed for evidence-based medicine information extraction tasks.

The dataset consists of approximately 5,000 clinical trial abstracts collected from biomedical literature. Each abstract is annotated at the token level with labels corresponding to key components of clinical studies. For this project, the following information categories are considered:

- **Participants (PART)** – the population or patient group involved in the study
- **Intervention (INT)** – the treatment, procedure, or action being evaluated
- **Outcome (OUT)** – the measured effects or results of the intervention

The dataset provides a benchmark for evaluating different information extraction approaches, ranging from rule-based methods to machine learning and large language model-based techniques.

**Dataset Repository:** https://github.com/bepnye/EBM-NLP

## Methodology

The project investigates multiple approaches for extracting structured information from clinical trial abstracts. The workflow consists of exploratory analysis, information extraction, and performance evaluation.

### 1. Exploratory Data Analysis

As a preliminary step, sentence-level embeddings were generated using the **SentenceTransformer (all-MiniLM-L6-v2)** model. Each clinical trial abstract was segmented into sentences, and embeddings were computed for each sentence.

To explore the structure of the embedding space, **K-means clustering** was applied to the sentence embeddings. The optimal number of clusters was determined using the **Within-Cluster Sum of Squares (WCSS) Elbow Method**, resulting in **K = 3**.

For visualization purposes, **Principal Component Analysis (PCA)** was used to project the embeddings into a two-dimensional space for qualitative inspection.

### 2. Rule-Based Approaches
Two rule-based methods were implemented:

- **Unigram, Bigram, and Regex Pattern Matching:** Extraction based on manually defined keyword patterns, n-gram matching, and regular expression-based rules designed to capture common lexical and structural patterns in clinical trial text.

- **spaCy Rule-Based Extraction:** Pattern matching using spaCy’s NLP pipeline and rule-based components for identifying relevant entities.

These approaches serve as interpretable baselines for information extraction. However, they are limited in their ability to generalise to unseen linguistic variations and complex contextual patterns in clinical abstracts.

### 3. Conditional Random Fields (CRF)

A Conditional Random Field (CRF) model was trained for sequence labelling. The model uses contextual and lexical features to predict entity labels for each token in an abstract.

CRFs are widely used for information extraction tasks because they model dependencies between neighbouring labels and can capture local contextual information.

### 4. Large Language Models (LLMs)

Prompt-based extraction was evaluated using a large language model - **LLaMA 3.2 3B Instruct (4-bit quantized)**, under two settings:

- **Zero-Shot Learning:** The model performs extraction using task instructions without labelled examples.
- **Few-Shot Learning:** A small number of annotated examples are provided within the prompt to guide extraction.

These approaches leverage the reasoning and language understanding capabilities of foundation models without task-specific training.

### 5. LoRA Fine-Tuning

Low-Rank Adaptation (LoRA) was used to fine-tune a pretrained language model- **LLaMA 3.2 3B Instruct (4-bit quantized)** on the EBM-NLP dataset.

LoRA introduces trainable low-rank matrices into selected transformer layers, enabling efficient domain adaptation while requiring significantly fewer trainable parameters than full model fine-tuning.

## Evaluation Metrics

Models were evaluated using standard Information Extraction (NER) metrics:

- **Precision:** Correctness of extracted entities  
- **Recall:** Coverage of true entities  
- **F1-score:** Primary metric combining precision and recall  

All metrics were computed using **seqeval**, a standard evaluation framework for sequence labeling tasks, ensuring strict span-level matching of predicted and ground-truth entities.

Evaluation was conducted at two levels:

- **Model-level (micro/macro averages):** Overall performance across all entity types  
- **Field-level:** Performance across individual categories (**PART**, **INT**, **OUT**)  
## Results
The performance of all models is summarised in the following tables.

### Model-Level Performance (Micro-Averaged)

| Model                         | Precision | Recall | F1-score |
|------------------------------|----------|--------|----------|
| Rule-based (Bigram) | 0.07     | 0.04   | 0.05     |
| spaCy Rule-based              | 0.14     | 0.18   | 0.15     |
| CRF                           | 0.41     | 0.21   | 0.28     |
| LLM Zero-shot                | 0.79     | 0.40   | 0.46     |
| LLM Few-shot                | 0.84     | 0.55   | 0.61     |
| LoRA Fine-tuned LLM         | 0.85     | 0.59   | 0.64     |

### Class-balanced Evaluation (Macro-Averaged Performance)

| Model                         | Macro Precision | Macro Recall | Macro F1 |
|------------------------------|----------------|--------------|----------|
| Rule-based (Unigram + Bigram) | 0.08           | 0.04         | 0.05     |
| spaCy Rule-based              | 0.12           | 0.18         | 0.13     |
| CRF                           | 0.38           | 0.20         | 0.26     |
| LLM Zero-shot                | 0.62           | 0.40         | 0.46     |
| LLM Few-shot                | 0.70           | 0.55         | 0.61     |
| LoRA Fine-tuned LLM         | 0.73           | 0.59         | 0.64     |

### Field-Level Performance (F1-score by Entity Type)

| Model                         | INT F1 | OUT F1 | PART F1 |
|------------------------------|--------|--------|----------|
| Rule-based (Unigram + Bigram) | 0.38   | 0.33   | 0.22     |
| spaCy Rule-based              | 0.25   | 0.04   | 0.11     |
| CRF                           | 0.36   | 0.25   | 0.16     |
| LLM Zero-shot                | 0.38   | 0.33   | 0.22     |
| LLM Few-shot                | 0.50   | 0.58   | 0.43     |
| LoRA Fine-tuned LLM         | 0.55   | 0.61   | 0.47     |

## Discussion

The experimental results demonstrate a clear performance hierarchy across the different information extraction strategies.

Large Language Model-based approaches consistently outperform traditional methods, with the LoRA fine-tuned model achieving the best overall performance across both micro-averaged and macro-averaged evaluations. This indicates that domain adaptation through fine-tuning significantly improves the model’s ability to extract structured biomedical information.

Few-shot learning also shows strong performance improvements over zero-shot prompting, particularly in recall and F1-score. This suggests that providing task-specific examples helps guide the model towards more accurate and complete extraction behaviour.

In contrast, rule-based approaches and spaCy-based methods perform poorly in terms of recall, despite achieving relatively higher precision in some cases. This highlights their limitation in generalising beyond predefined patterns and their inability to capture the variability present in clinical trial language.

The CRF model provides a moderate baseline, outperforming rule-based systems but significantly lagging behind LLM-based methods. While CRFs are effective at capturing local dependencies in structured sequence labelling tasks, they struggle with long-range contextual understanding.

Across all models, the PART (Outcome) category remains the most challenging to extract. This is likely due to the variability and complexity of outcome expressions in clinical trial abstracts compared to more structured participant and intervention descriptions.

Overall, the results indicate that contextual language understanding plays a critical role in structured biomedical information extraction, with fine-tuned LLMs offering the most robust and balanced performance across all evaluation settings.

## Limitations

This study has several limitations related to evaluation metrics, dataset characteristics, and modelling approaches.

Firstly, standard evaluation metrics such as precision, recall, and F1-score have inherent limitations. Precision does not account for false negatives, while recall ignores false positives, meaning each metric captures only one aspect of model performance. Although F1-score provides a balance between the two, it assumes equal importance of precision and recall, which may not reflect real-world biomedical information extraction requirements.

Additionally, macro-averaged F1-score can be misleading in imbalanced datasets, as it assigns equal importance to all classes and may overemphasise performance on rare entity types while underrepresenting dominant ones.

In this study, evaluation was performed using the seqeval framework, which follows a strict span-based (BIO) evaluation scheme. This approach only rewards predictions when the entire entity span exactly matches the ground truth. As a result, partially correct spans receive no credit, even if they are semantically close to the correct annotation. This strict matching criterion can lead to lower scores and does not capture partial or near-miss correctness.

From a modelling perspective, rule-based systems are limited by lexical rigidity and cannot generalise beyond predefined patterns. CRF models are constrained by boundary prediction errors, particularly in identifying accurate entity spans. Large language models may generate inconsistent outputs or hallucinated entities, especially in complex biomedical contexts. While LoRA fine-tuning improves performance, it still struggles with rare classes due to limited training examples.

Finally, all experiments are conducted on clinical trial abstracts only, which limits generalisation to full-text biomedical documents where linguistic structures are more complex and varied.

## Downstream Usability

Downstream usability refers to how effectively the extracted entities can be utilised for tasks such as querying, structured storage, and further analysis.

In this study, all extracted outputs were converted into a structured CSV format to facilitate easy querying and integration into downstream analytical workflows.

Rule-based methods often produce incomplete or fragmented extractions, which limits their usefulness in downstream applications. CRF-based models improve consistency in predictions but still suffer from boundary detection errors, which can affect the reliability of structured outputs.

Large Language Model-based approaches generate more coherent and contextually complete extractions, improving their suitability for downstream use. Among all approaches, the LoRA fine-tuned model produces the most complete and reliable structured outputs, resulting in the highest downstream usability for practical biomedical information extraction tasks.

## Conclusion

This project investigated multiple approaches for structured information extraction from clinical trial abstracts using the EBM-NLP dataset. The task focused on extracting Participants (PART), Intervention (INT), and Outcome (OUT) information from unstructured biomedical text and evaluating different modelling strategies.

A range of methods were implemented, including rule-based approaches, Conditional Random Fields (CRF), large language models (zero-shot and few-shot prompting), and a LoRA fine-tuned LLM. The results show a clear progression in performance from traditional methods to more advanced language model-based approaches.

Overall, the LoRA fine-tuned model achieved the best performance across both micro and macro evaluations, demonstrating the effectiveness of domain-specific adaptation for biomedical information extraction. Few-shot and zero-shot LLM approaches also significantly outperformed rule-based and CRF methods, highlighting the importance of contextual understanding in clinical text.

In contrast, rule-based and CRF methods served as useful baselines but were limited by poor generalisation and boundary prediction errors. Across all models, extracting Outcome (PART/OUT-related) information remained the most challenging task due to variability in clinical language.

In conclusion, the study demonstrates that modern LLM-based approaches, particularly fine-tuned models, provide the most robust solution for structured extraction from clinical trial abstracts, with clear advantages in usability, completeness, and overall extraction quality.
