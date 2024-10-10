# Financial-Text-Sentiment-Analysis

## Objective
The objective of this document is to explain the methodology adopted to perform text analysis to derive sentiment opinions, sentiment scores, readability metrics, and other linguistic features.

## Table of Contents
1. [Sentiment Analysis](#sentiment-analysis)
   - [Cleaning using Stop Words Lists](#cleaning-using-stop-words-lists)
   - [Creating a Dictionary of Positive and Negative Words](#creating-a-dictionary-of-positive-and-negative-words)
   - [Extracting Derived Variables](#extracting-derived-variables)
2. [Analysis of Readability](#analysis-of-readability)
3. [Average Number of Words Per Sentence](#average-number-of-words-per-sentence)
4. [Complex Word Count](#complex-word-count)
5. [Word Count](#word-count)
6. [Syllable Count Per Word](#syllable-count-per-word)
7. [Personal Pronouns](#personal-pronouns)
8. [Average Word Length](#average-word-length)

## Sentiment Analysis

Sentiment analysis is the process of determining whether a piece of writing is positive, negative, or neutral. The algorithm consists of the following steps:

### Cleaning using Stop Words Lists
Utilizes the Stop Words Lists (located in the `StopWords` folder) to clean the text, excluding words found in the stop words list.

### Creating a Dictionary of Positive and Negative Words
Uses the Master Dictionary (found in the `MasterDictionary` folder) to create a dictionary of positive and negative words, adding only those not found in the Stop Words Lists.

### Extracting Derived Variables
Converts the text into a list of tokens using the NLTK tokenize module and calculates the following variables:

- **Positive Score**: Sum of +1 for each word in the Positive Dictionary.
- **Negative Score**: Sum of -1 for each word in the Negative Dictionary (multiplied by -1 for positive representation).
- **Polarity Score**: 
  \[
  \text{Polarity Score} = \frac{\text{Positive Score} - \text{Negative Score}}{\text{Positive Score} + \text{Negative Score} + 0.000001}
  \]
  (Range: -1 to +1)

- **Subjectivity Score**:
  \[
  \text{Subjectivity Score} = \frac{\text{Positive Score} + \text{Negative Score}}{\text{Total Words after cleaning} + 0.000001}
  \]
  (Range: 0 to +1)

## Analysis of Readability
Readability is calculated using the Gunning Fog index:

- **Average Sentence Length**: Number of words / Number of sentences
- **Percentage of Complex Words**: Number of complex words / Number of words
- **Fog Index**:
  \[
  \text{Fog Index} = 0.4 \times \left(\text{Average Sentence Length} + \text{Percentage of Complex Words}\right)
  \]

## Average Number of Words Per Sentence
\[
\text{Average Number of Words Per Sentence} = \frac{\text{Total Number of Words}}{\text{Total Number of Sentences}}
\]

## Complex Word Count
Complex words are defined as those containing more than two syllables.

## Word Count
1. Remove stop words using the NLTK package.
2. Remove punctuation (e.g., `?`, `!`, `,`, `.`) before counting.

## Syllable Count Per Word
Counts the number of syllables in each word by counting vowels, handling exceptions for words ending in "es" or "ed".

## Personal Pronouns
Counts occurrences of personal pronouns ("I," "we," "my," "ours," "us") using regex, ensuring the country name "US" is excluded.

## Average Word Length
\[
\text{Average Word Length} = \frac{\text{Sum of characters in each word}}{\text{Total Number of Words}}
\]

## Code Snippet

```python
import nltk
from nltk.corpus import stopwords
import re

def clean_text(text):
    # Clean text by removing stop words and punctuation
    stop_words = set(stopwords.words('english'))
    words = nltk.word_tokenize(text)
    cleaned_words = [word for word in words if word.lower() not in stop_words and word.isalpha()]
    return cleaned_words

def sentiment_analysis(text):
    positive_words = set(...)  # Load positive words
    negative_words = set(...)  # Load negative words
    cleaned_words = clean_text(text)
    
    positive_score = sum(1 for word in cleaned_words if word in positive_words)
    negative_score = sum(1 for word in cleaned_words if word in negative_words)
    
    polarity_score = (positive_score - negative_score) / (positive_score + negative_score + 0.000001)
    subjectivity_score = (positive_score + negative_score) / (len(cleaned_words) + 0.000001)
    
    return positive_score, negative_score, polarity_score, subjectivity_score

# Further functions for readability analysis, word count, syllable count, etc., would go here.
