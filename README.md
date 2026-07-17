# Next Word Prediction with LSTM/GRU

A deep-learning project that predicts the next word in a text sequence using a recurrent neural network trained on William Shakespeare's *Hamlet*. The repository includes the training notebook, trained model files, tokenizer, dataset, and a Streamlit web application for interactive predictions.

## Overview

The project follows a complete natural language processing workflow:

1. Load the *Hamlet* text from the NLTK Gutenberg corpus.
2. Convert the text to lowercase and tokenize it into integer sequences.
3. Generate n-gram training examples from each line of text.
4. Pad all sequences to a fixed length.
5. Train a recurrent neural network to predict the next word.
6. Apply early stopping to reduce unnecessary training and overfitting.
7. Save the trained model and tokenizer.
8. Serve predictions through a Streamlit application.

## Features

- Next-word prediction from user-entered text
- Text preprocessing with the Keras `Tokenizer`
- Sequence padding for fixed-length model inputs
- Embedding and recurrent neural-network layers
- LSTM and GRU model experiments
- Early stopping based on validation loss
- Saved model and tokenizer for inference
- Simple Streamlit user interface

## Model Architecture

The notebook contains experiments with both LSTM and GRU architectures. Each architecture uses:

- An embedding layer with 100-dimensional word vectors
- Two recurrent layers with 150 and 100 units
- A dropout layer with a rate of `0.2`
- A dense output layer with softmax activation
- Categorical cross-entropy loss
- The Adam optimizer

The processed dataset contains approximately `4,818` vocabulary entries, and the maximum full sequence length is `14` tokens. The model therefore receives up to `13` input tokens and predicts the next token.

> **Model naming note:** The Streamlit application loads `next_word_lstm.h5`. In the notebook, an LSTM model is defined first and a GRU model is defined afterward. When the notebook is executed from top to bottom, the GRU model replaces the LSTM model variable before training. Rename the files or adjust the notebook if you want the saved model name to exactly match the trained architecture.

## Project Structure

```text
.
├── app(2).py
├── experiemnts.ipynb
├── hamlet.txt
├── next_word_lstm.h5
├── next_word_lstm_model_with_early_stopping.h5
├── tokenizer.pickle
├── requirements(4).txt
└── README.md
```

### File Descriptions

| File | Description |
|---|---|
| `app(2).py` | Streamlit application used to enter text and predict the next word |
| `experiemnts.ipynb` | Notebook containing data preparation, model development, training, evaluation, and artifact saving |
| `hamlet.txt` | Training text containing Shakespeare's *Hamlet* |
| `next_word_lstm.h5` | Model loaded by the Streamlit application |
| `next_word_lstm_model_with_early_stopping.h5` | Additional trained model artifact associated with early stopping |
| `tokenizer.pickle` | Saved Keras tokenizer used to convert text into token IDs |
| `requirements(4).txt` | Python dependencies required by the project |

## Installation

### 1. Create a virtual environment

```bash
python -m venv .venv
```

Activate it on macOS or Linux:

```bash
source .venv/bin/activate
```

Activate it on Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

### 2. Install the dependencies

```bash
pip install --upgrade pip
pip install -r "requirements(4).txt"
```

Use a Python environment that is compatible with TensorFlow `2.15.0`.

## Run the Streamlit Application

Run the following command from the project directory:

```bash
streamlit run "app(2).py"
```

Streamlit will display a local URL in the terminal. Open that URL in your browser, enter a sequence such as:

```text
To be or not to
```

Then select **Predict Next Word** to see the model's prediction.

## Train the Model

Open the notebook:

```bash
jupyter notebook experiemnts.ipynb
```

Run the cells in order to:

1. Download and save the NLTK Gutenberg version of *Hamlet*.
2. Create the tokenizer and vocabulary.
3. Generate and pad input sequences.
4. Split the data into training and validation sets.
5. Build the recurrent model.
6. Train it with early stopping.
7. test example predictions.
8. Save the model and tokenizer.

The notebook configures early stopping with:

```python
EarlyStopping(
    monitor="val_loss",
    patience=3,
    restore_best_weights=True,
)
```

## Inference Process

For each prediction, the application:

1. Converts the input text to token IDs using the saved tokenizer.
2. Keeps only the most recent tokens when the input is too long.
3. Pads the sequence to the model's expected input length.
4. Produces a probability distribution over the vocabulary.
5. Selects the word with the highest probability.
6. Converts the predicted token ID back into a word.

## Example

```text
Input:  To be or not to be
Output: considered
```

The exact prediction can change when the model is retrained because of random initialization and data splitting.

## Limitations

- The model is trained only on *Hamlet*, so its vocabulary and writing style are strongly Shakespearean.
- It predicts only one most-likely word rather than multiple ranked suggestions.
- Words not present in the tokenizer vocabulary may be ignored.
- A relatively small literary dataset limits generalization to modern language.
- The model does not understand meaning in the same way as a modern large language model.

## Possible Improvements

- Add top-k predictions with probability scores.
- Generate several words recursively instead of only one word.
- Train on a larger and more modern text corpus.
- Compare LSTM and GRU performance using consistent model names.
- Add reproducible random seeds and evaluation metrics.
- Improve the Streamlit interface with prediction history and configurable generation length.
- Convert the model to the newer Keras format, such as `.keras`.

## Acknowledgements

The training text is based on William Shakespeare's *Hamlet* as distributed through the NLTK Gutenberg corpus.
