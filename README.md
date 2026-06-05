## Model-Level Performance Comparison

| Model | Precision | Recall | F1-score |
|------|-----------|--------|----------|
| Rule-based (Unigram + Bigram) | 0.07 | 0.04 | 0.05 |
| spaCy Rule-based | 0.14 | 0.18 | 0.15 |
| CRF | 0.41 | 0.21 | 0.28 |
| LLM Zero-shot | 0.79 | 0.40 | 0.46 |
| LLM Few-shot | 0.84 | 0.55 | 0.61 |
| LoRA Fine-tuned LLM | 0.85 | 0.59 | 0.64 |

## Field-Level Performance Comparison (F1-score)

| Model | INT | OUT | PART |
|------|-----|-----|------|
| Rule-based (Unigram + Bigram) | 0.38 | 0.33 | 0.22 |
| spaCy Rule-based | 0.25 | 0.04 | 0.11 |
| CRF | 0.36 | 0.25 | 0.16 |
| LLM Zero-shot | 0.38 | 0.33 | 0.22 |
| LLM Few-shot | 0.50 | 0.58 | 0.43 |
| LoRA Fine-tuned LLM | 0.55 | 0.61 | 0.47 |
