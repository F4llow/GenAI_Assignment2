# Assignment 2: Summarizing Code via LSTM Models

**Course:** CSCI 455/555: GenAI for Software Development  
**Date:** March 16, 2026 (Extended)  
**Instructor:** Prof. Antonio Mastropaolo  
**Student:** Nathaniel Callabresi  

---

## Project Overview

The goal of this project is to implement an LSTM-based Sequence-to-Sequence (Seq2Seq) model capable of generating concise natural language summaries for Java methods. By leveraging pre-trained **CodeT5+** embeddings, the model maps complex code structures to human-readable documentation.

---

## Data Source(s) and Pre-processing

### Corpus Construction
- Mined public GitHub repositories to construct a custom dataset of Java methods.
- **Dataset Size:** ~50,000 training pairs, 1,000 validation samples, and 1,000 test samples.

### Pre-processing Pipeline
- **Flattening:** Java methods were stripped of newlines and normalized to single whitespace lines.
- **Normalization:** All summaries were converted to lowercase to reduce vocabulary sparsity.
- **Filtering:** Applied quality filters to remove non-ASCII characters and outlier samples with extreme token lengths.
- **Tokenization:** Utilized the `Salesforce/codet5p-220m` tokenizer for consistent embedding alignment.

---

## Model Architecture

The model follows a classic Encoder-Decoder architecture implemented in PyTorch:

- **Embeddings:** A pre-trained **32,100 × 768** matrix from CodeT5+, frozen during training to preserve semantic knowledge.
- **Encoder:** A 2-layer Bi-LSTM with a hidden dimension of 256 and 20% dropout.
- **Decoder:** A 2-layer LSTM that uses **Teacher Forcing** during training to predict the next token in the sequence.
- **Early Stopping:** Training is governed by the **BLEU-1 score** on the validation set. If BLEU-1 does not improve for **3 epochs**, training is terminated to prevent overfitting.

---

## Evaluation Results

The final model was evaluated on the created test set using standard NLP metrics and the **SIDE** (Summary alIgnment to coDe sEmantics) metric developed specifically for code summarization. For evaluating using the provided test set, please uncomment the lines in cell 10. To make the notebook easier to run completely through, I did not want the program to rely on this external test set being present.

Here are the scores of the best model I found:

| Metric | Score |
| :--- | :--- |
| **BLEU-1** | 27.12 |
| **BLEU-2** | 19.09 |
| **BLEU-3** | 14.64 |
| **BLEU-4** | 11.91 |
| **METEOR** | 0.2412 |
| **BERTScore (F1)** | 0.8572 |
| **SIDE Score** | 0.6623 |

---
## Installation & Reproducibility

### 1. Prerequisites
- **Python 3.9** (Required for compatibility with specific library versions used in this environment).

### 2. Install Dependencies
```bash
pip install -r requirements.txt
```

### 3. Prepare the SIDE Metric
Ensure the fine-tuned SIDE model weights are located in the `./side_model/` directory. These are required for the semantic alignment evaluation. 
- **Model Weights:** The folder can be downloaded from [this Google Drive link](https://drive.google.com/drive/folders/1QdxrYnelt9poi45eLT5xgObihDRb_OtV). 
- **Background:** This is a fine-tuned SIDE model created by Antonio Mastropaolo, detailed in the [official repository](https://github.com/antonio-mastropaolo/code-summarization-metric).

### 3. Execution
Run the `assignment-2-LSTM.ipynb` notebook. The script will:
1. Load the pre-processed `.pt` dataset.
2. Initialize the LSTM model with frozen CodeT5+ embeddings.
3. Execute the training loop with BLEU-1 monitoring.
4. Generate final predictions and compute all seven evaluation metrics.

---

## Outputs

- `best_lstm_model.pt` — The saved weights of the best-performing model based on validation BLEU-1.
- `assignment2_predictions.json` — A JSON file containing method IDs, original code, human references, and the model's generated predictions.
