
# Implementation-of-Auto-correct-and-Minimum-Edit-Distance

## AIM:
To implement an Auto-Correct System in Python using the Minimum Edit Distance algorithm and word frequency analysis from the NLTK corpus.

## THEORY:
Auto-correct systems aim to detect and correct misspelled words. One of the most effective approaches to this problem is using the Minimum Edit Distance (Levenshtein Distance), which measures how many single-character edits (insertions, deletions, or substitutions) are needed to change one word into another.

**Key Concepts:**
Minimum Edit Distance: Measures similarity between words based on edit operations.

NLTK Words Corpus: Acts as a vocabulary of correctly spelled English words.

Word Probability: Ranks suggested words by their likelihood using word frequency.

Correction Algorithm: Suggests the most probable correction among candidates with the lowest edit distance.

## PROCEDURE:
Import Libraries: Import NLTK and other required modules.

Download and Load Corpus: Use nltk.corpus.words as the vocabulary.

Define Edit Distance Function: Implement the dynamic programming approach for edit distance.

Calculate Word Probability: Use frequency count from the corpus.

Correction Logic:

Check if a word is correctly spelled.

Generate candidates within edit distance 1 or 2.

Select the candidate with the highest probability.

Test the System: Use common misspelled words as input and verify outputs.

## PROGRAM:
```python

import nltk
nltk.download('words')
from nltk.corpus import words
import re
from collections import Counter

# Use the NLTK words corpus as our vocabulary
word_list = words.words()
word_freq = Counter(word_list)
WORD_SET = set(word_list)

# Minimum Edit Distance function
def edit_distance(word1, word2):
    dp = [[0] * (len(word2) + 1) for _ in range(len(word1) + 1)]
    for i in range(len(word1) + 1):
        for j in range(len(word2) + 1):
            if i == 0:
                dp[i][j] = j
            elif j == 0:
                dp[i][j] = i
            elif word1[i - 1] == word2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]
            else:
                dp[i][j] = 1 + min(dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1])
    return dp[-1][-1]

# Word probability function
def word_probability(word, N=sum(word_freq.values())):
    return word_freq[word] / N if word in word_freq else 0

# Auto-correct function
def autocorrect(word):
    if word in WORD_SET:
        return word
    candidates = [w for w in WORD_SET if edit_distance(word, w) <= 2]
    corrected_word = max(candidates, key=word_probability, default=word)
    return corrected_word

# Test the function
test_words = ["speling", "korrect", "exampl", "wrld"]
for word in test_words:
    print(f"Original: {word} -> Suggested: {autocorrect(word)}")
```
## OUTPUT:


## RESULT:
Thus, the implementation of an Auto-Correct System using Minimum Edit Distance was successfully achieved. The system effectively suggested the correct spellings by computing edit distances and evaluating word probabilities using the NLTK corpus.
