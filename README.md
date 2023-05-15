# Spell Correction for Roman Urdu

This project implements spell correction for Roman Urdu using the Noisy Channel model. The Noisy Channel model is a widely used technique for correcting non-word errors, and is used by word processors as well as Google in its search engine.

## Background

The spell correction program for Roman Urdu is implemented in Python and is based on the Noisy Channel model, which is based on Bayes Theorem. The corrected word is obtained by performing an argmax over all candidate words to pick the word with the highest probability.

## Data

The project requires two files, `data.txt` and `misspellings.txt`. `data.txt` is a collection of Roman Urdu sentences and `misspellings.txt` contains a list of words and their misspellings.

## Implementation Details

The spell corrector has four main components:

1. Language Model P(w): A simple unigram model is trained using the provided file `data.txt` and any other corpus that can be found. The language model gives the probability of each word occurring in the vocabulary V.
2. Error Model P(x|w): This model estimates the probability of typing x when w was intended. This error is modeled using character-level insert, delete, transpose, and substitute tables. The tables are generated using the provided list of common misspellings in Roman Urdu and any other list that can be found.
3. Candidate Words Generation (w ∈ candidates): The candidate model generates all the possible words w that could have been mistyped as x. This includes words that are more than one edit away from x and exist in the vocabulary V determined by `data.txt`.
4. Selection Model using argmax: An ensemble of word-level and character-level language models is used to score candidate correct sentences. The final selection using argmax chooses the candidate corrected sentence ˆw that has the highest probability specified by the ensemble of language models.

### The Need for Generating Error Model Edit Tables

Generating possible combinations for correcting a misspelled word is not enough. You also need to know which of those combinations are actual words and how probable the misspelling is given the correct word. This is where the language model and the error model come into play. The tables are used to calculate P(x|w).

### How to Generate Error Model Edit Tables

The tables are created using misspellings.txt. This text file contains a list of words, the first word in each line is the correct word and after the comma are all its common misspellings which are one edit away. You have to find what type of edit has been done from insert, delete, substitute, and transpose and populate the corresponding table for each misspelled word. Insertion and deletion are conditioned on the previous character.
