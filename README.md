# Assignment 2: Summarizing Code via LSTM Models

**Course:** CSCI 455/555: GenAI for Software Development  
**Date:** March 16, 2026 (Extended)  
**Instructor:** Prof. Antonio Mastropaolo  
**Student:** Nathaniel Callabresi  

---

## Project Overview

The goal of this project is to implement an LSTM model that can generate natural language summaries for Java methods. The system utilizes a Seq2Seq architecture with pre-trained embeddings to understand code semantics.

---

## Data Source(s) and Pre-processing

### Corpus Construction
- Mined public GitHub repositories to construct a custom dataset of Java methods.

### Requirements
- Built a dataset consisting of ~50,000 code-summary pairs for training and 1,000 samples for validation.

### Pre-processing
- Java methods were flattened into single whitespace-normalized lines.
- All summaries were converted to lowercase.
- Quality filters were applied to remove non-ASCII characters and limit token counts to ensure high-quality training data.

---

## Installation & Reproducibility

To reproduce the model training and evaluation:

### 1. Install Dependencies

```bash
pip install -r requirements.txt
```

### 2. Generate Embeddings

Run the provided `get_codet5_embeddings.py` script to tokenize the data and extract the CodeT5+ embedding matrix:

```bash
python get_codet5_embeddings.py --input <source_file>.txt --output <output_file>.pt
```

### 3. Run Notebook

Execute the `assignment-2-LSTM.ipynb` notebook from top to bottom.

---

## Model Architecture

- **Architecture:** LSTM encoder-decoder following the course notebook structure.
- **Decoder:** Adapted to generate natural language tokens instead of code tokens.
- **Embeddings:** Uses a **32,100 × 768** pre-trained embedding matrix from CodeT5+.
- **Training Logic:** Includes early stopping based on the **BLEU-1 score** computed on the validation set.

---

## Evaluation Metrics

The final model is evaluated on the test set using the following metrics:

- **BLEU-1**
- **BLEU-2**
- **BLEU-3**
- **BLEU-4**
- **METEOR**
- **BERTScore**
- **SIDE**

---

## Outputs

Outputs are saved in the main directory:

- `best_lstm_model.pt` — Model checkpoint with the highest validation BLEU-1.
- `assignment2_predictions.json` — Generated summaries for the instructor-provided test set.
