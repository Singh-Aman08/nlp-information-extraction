
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
