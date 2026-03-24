
---

# 🧠 Text Vectorization Tutorial (Complete End-to-End Guide)

---

# 🔰 STEP 0: Import Libraries

```python
import numpy as np
from sklearn.feature_extraction.text import CountVectorizer, TfidfVectorizer
import pandas as pd
```

### 📌 Explanation

* `numpy` → numerical operations
* `pandas` → table format (DataFrame)
* `CountVectorizer` → Bag of Words
* `TfidfVectorizer` → TF-IDF

---

# 🔰 STEP 1: Create Corpus

```python
corpus = [
    "i love nlp nlp nlp",
    "i teach gen ai gen ai gen ai",
    "i am working working with eiker eiker"
]

print("Corpus:", corpus)
```

### 📌 Concept: Corpus

A **corpus** is a collection of text documents.

👉 Each sentence = 1 document
👉 Entire list = dataset

---

# 🔰 STEP 2: Join Corpus

```python
text = " ".join(corpus)
print("Joined text:", text)
```

### 📌 Why?

To combine all text for:

* extracting words
* building vocabulary

---

# 🔰 STEP 3: Tokenization

```python
words = text.split()
print("All words:", words)
```

### 📌 Concept: Tokenization

Breaking text into **words (tokens)**

---

# 🔰 STEP 4: Unique Words (Vocabulary)

```python
unique_words = list(set(words))
print("Unique words:", unique_words)
print("Vocabulary size:", len(unique_words))
```

### 📌 Concept: Vocabulary

All **unique words** in corpus

---

# 🔰 STEP 5: Word → Index Mapping

```python
word_to_index = {word: index for index, word in enumerate(unique_words)}
print("Word to index mapping:", word_to_index)
```

### 📌 Why?

Convert words → numbers (machine understanding)

---

# 🔴 STEP 6: One-Hot Encoding

---

## 📌 Concept

**One-Hot Encoding**:
Each word is represented by a vector of zeros, with a single '1' at the index corresponding to the word's position in the vocabulary.

---

## 📌 Sparse Matrix Explanation

A **sparse matrix** is a matrix in which most of the elements are zero.

In the context of one-hot encoding:

* Each row of the matrix corresponds to a sentence
* Each column corresponds to a unique word in the corpus
* The value:

  * **1 → word present**
  * **0 → word absent**

---

## 📌 Code Implementation (Manual)

```python
hot_encoded = []

for sentence in corpus:
    sentence_vector = []  # this will hold the hot encoded vectors for each word in the sentence
    
    for word in sentence.split():
        vector = [0] * len(unique_words)
        # create vector same length as vocabulary
        
        index = word_to_index[word]
        # get index of word
        
        vector[index] = 1
        # mark presence
        
        sentence_vector.append(vector)
    
    hot_encoded.append(sentence_vector)
```

---

## 📌 Example Output

```python
print("Hot encoded example:", hot_encoded[0][0])
print("Shape:", len(hot_encoded), "sentences,", len(hot_encoded[0]), "words each")
```

---

## ⚠️ Limitations

* ❌ High memory usage
* ❌ Sparse matrix problem
* ❌ No semantic meaning

---

# 🔵 STEP 7: Bag of Words (BoW)

---

## 📌 Definition

**Bag of Words (BoW)** is a simple and commonly used text representation technique in **Natural Language Processing**.

It represents a text document as a vector of word frequencies, where each element corresponds to a unique word.

---

## 📌 Key Idea

👉 Count how many times each word appears

---

## 📌 Code

```python
vectorizer = CountVectorizer(vocabulary=unique_words)
X = vectorizer.fit_transform(corpus)
```

---

## 📌 Output

```python
print("Feature names:", vectorizer.get_feature_names_out())
print("BoW matrix:")
print(X.toarray())
```

---

## 📌 Example

```
"i love nlp nlp nlp"
→ i=1, love=1, nlp=3
```

---

## ⚠️ Limitations

* ❌ No word order
* ❌ No context
* ❌ No semantic meaning

---

# 🟢 STEP 8: TF-IDF

---

## 📌 Definition

**TF-IDF** is a statistical measure that evaluates the importance of a word in a document relative to a collection of documents (corpus).

It is calculated using:

* Term Frequency (TF)
* Inverse Document Frequency (IDF)

---

## 📌 Term Frequency (TF)

[
TF = \frac{\text{number of times a term appears in a document}}{\text{total number of terms in that document}}
]

---

## 📌 Inverse Document Frequency (IDF)

IDF = \log\left(\frac{N}{df}\right)

Where:

* N = total documents
* df = number of documents containing the word

---

## 📌 Final Formula

[
TF\text{-}IDF = TF \times IDF
]

---

## 📌 Code

```python
vectorizer_tfidf = TfidfVectorizer(vocabulary=unique_words)
X_tfidf = vectorizer_tfidf.fit_transform(corpus)
```

---

## 📌 Output

```python
print("TF-IDF matrix:")
print(X_tfidf.toarray())
```

👉 Values between **0 and 1**
👉 Higher = more important word

---

# 🔰 STEP 9: Convert to DataFrame

```python
word_importance = pd.DataFrame(
    X_tfidf.toarray(),
    columns=vectorizer_tfidf.get_feature_names_out()
)
```

---

## 📌 Why?

* Easier visualization
* Better analysis

---

## 📌 View Data

```python
print(word_importance)
```

---

# 🔰 STEP 10: Rank Important Words

```python
print(word_importance.sum(axis=0).sort_values(ascending=False))
```

👉 Shows most important words across corpus

---

# 🧠 FINAL COMPARISON

| Method  | What it does   | Output          | Problem      |
| ------- | -------------- | --------------- | ------------ |
| One-Hot | Presence (0/1) | Sparse vector   | No meaning   |
| BoW     | Frequency      | Count vector    | No context   |
| TF-IDF  | Importance     | Weighted vector | No semantics |

---

# 🚀 FINAL INTUITION

* One-Hot → “Word exists?”
* BoW → “How many times?”
* TF-IDF → “How important?”

---

# 🔥 BONUS: WHY THIS MATTERS

These are **foundation techniques in Natural Language Processing**

Used in:

* Search engines
* Spam detection
* Text classification

---


