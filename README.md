# ST1504 Deep Learning — Assignment 1 (CA1)
**Singapore Polytechnic · School of Computing · DAAA**  
**Module:** ST1504 Deep Learning · AY2026/27 Semester 1
 
---
 
## Overview
 
This project consists of two deep learning models built from scratch using Python and Keras/TensorFlow:
 
- **Part A** — Convolutional Neural Network (CNN) for handwritten letter classification
- **Part B** — Recurrent Neural Network (RNN) for pizza review sentiment analysis
---
 
## Part A — CNN Image Classification
 
### Dataset
- **EMNIST Letters** — 88,800 handwritten letter images (28×28 grayscale)
- 26 classes (A–Z), approximately 3,400 samples per class
- Loaded from `emnist-letters.csv`
### Approach
1. **EDA** — Class distribution, pixel intensity histogram, sample visualisation, intra-class variation analysis
2. **Preprocessing** — Normalisation (/255), EMNIST transpose fix, 70/15/15 stratified split
3. **Baseline CNN** — 2 conv blocks (32→64 filters), MaxPooling, Dense(128), Softmax
4. **Data Augmentation** — Rotation (±10°), width/height shift (10%), zoom (10%), no horizontal flip
5. **Improved CNN** — 3 conv blocks (64→128→256), BatchNormalization, Dropout, ReduceLROnPlateau
### Results
 
| Model | Test Accuracy | Test Loss | Error Rate |
|---|---|---|---|
| Baseline CNN | 92.67% | 0.2490 | 7.33% |
| Improved CNN | **95.67%** | 0.1230 | 4.33% |
 
**Improvement: +3.00%**
 
### Key Findings
- Data augmentation confirmed beneficial (+3% accuracy improvement)
- Overfitting reduced — train/val gap dropped from ~4% to ~0.4%
- I and L remain the hardest classes (F1 ~0.76) due to visual similarity in handwriting
---
 
## Part B — RNN Sentiment Analysis
 
### Dataset
- **Pizza Reviews** — 902 reviews with scores (0–10) in English and Malay
- Loaded from `Pizza_reviews.csv`
### Approach
1. **EDA** — Language distribution, score cleaning, label assignment, sample review analysis
2. **Decision** — Binary classification (Negative/Positive) over regression or 3-class due to noisy labels and small dataset size
3. **Language decision** — Kept English (492) + Malay (402); discarded 8 rows of other languages
4. **Feature Engineering** — Sentence splitting (+38% more data), text cleaning, Keras Tokenizer, padding (MAX_LEN=50)
5. **Baseline RNN** — Embedding(32, mask_zero) → LSTM(64) → Dense(32) → Softmax
6. **Improved RNN** — Embedding(64, mask_zero) → Dropout → BiLSTM(64) → Dropout → LSTM(32) → Dense → Softmax
### Results
 
| Model | Test Accuracy | Test Loss |
|---|---|---|
| Baseline RNN | ~95.73% | ~0.13 |
| Improved RNN | ~95.12% | ~0.14 |
 
### Key Findings
- Binary classification was the right choice — 3-class attempt failed due to noisy/contradictory labels
- Sentence splitting increased training data from ~877 to ~1,211 samples (+38%)
- Class weighting was essential — without it, model predicted only the majority class
- Bidirectional LSTM improved coverage of sentiment words across the full review
- Limitation: English and Malay tokenized independently — semantically similar words treated as unrelated
---
 
## Project Structure
 
```
ST1504_CA1/
├── PartA/
│   ├── ST1504_CA1_PartA.ipynb          # CNN notebook
│   ├── ST1504_CA1_PartA.html           # HTML export
│   └── best_improved_weights.h5        # Best CNN weights
├── PartB/
│   ├── ST1504_RNN_Assignment_CA1.ipynb # RNN notebook
│   ├── ST1504_RNN_Assignment_CA1.html  # HTML export
│   └── best_improved_rnn_weights.h5   # Best RNN weights
├── slides.pptx                         # Presentation slides
├── Declaration_of_Academic_Integrity.pdf
└── README.md
```
 
---
 
## Requirements
 
```
Python 3.10+
tensorflow >= 2.x
numpy
pandas
matplotlib
seaborn
scikit-learn
```
 
---
 
## How to Run
 
1. Clone or unzip the project folder
2. Place `emnist-letters.csv` in the same directory as `ST1504_CA1_PartA.ipynb`
3. Place `Pizza_reviews.csv` in the same directory as `ST1504_RNN_Assignment_CA1.ipynb`
4. Open each notebook in Jupyter and run **Kernel → Restart & Run All**
5. Best weights are saved automatically as `.h5` files during training
---
 
## Tech Stack
 
| Tool | Purpose |
|---|---|
| Python | Core language |
| TensorFlow / Keras | Model building and training |
| NumPy / Pandas | Data manipulation |
| Matplotlib / Seaborn | Visualisation |
| Scikit-learn | Train/val/test split, metrics |
 
---
 
## References
 
- Cohen, G., et al. (2017). EMNIST: Extending MNIST to handwritten letters. *IJCNN 2017*.
- LeCun, Y., et al. (1998). Gradient-based learning applied to document recognition. *Proceedings of the IEEE, 86*(11), 2278–2324.
- Hochreiter, S., & Schmidhuber, J. (1997). Long short-term memory. *Neural Computation, 9*(8), 1735–1780.
- Goldberg, Y. (2017). *Neural Network Methods for Natural Language Processing*. Morgan & Claypool.
- Keras Documentation: https://keras.io
---
 
*Singapore Polytechnic · School of Computing · AY2026/27*
