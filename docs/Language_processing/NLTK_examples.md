# Working with NLTK

## Word vectorization

- [Word vectorization](https://www.analyticsvidhya.com/blog/2021/06/part-5-step-by-step-guide-to-master-nlp-text-vectorization-approaches/)

A neural networks can only work with numbers. To use text as input we need to convert it to numbers. This is called vectorization. There are several ways to vectorize text. The most common are: count vectorizer, bag of words and n-grams.

### Count vectorizer

It creates a document term matrix, which is a set of dummy variables that indicates if a particular word appears in the document

Count vectorizer will fit and learn the word vocabulary and try to create a document term matrix in which the individual cells denote the frequency of that word in a particular document, which is also known as term frequency, and the columns are dedicated to each word in the corpus.

### Bag of words

Bag of words is a more advanced way to vectorize text. It creates a vector with the number of unique words in the text as the length of the vector. Each element in the vector corresponds to a word in the text. The value of the element is the number of times the word appears in the text.


## Parallel vs non-parallel corpora / supervised vs unsupervised

Text Style Transfer algorithms ca be developed in a supervised way, using parallel corpora, i.e a corpus of text in the source language and a corpus of text in the target language. The algorithm is trained to map the source text to the target text. For unsupervised style transfer, we can use non-parallel corpora, i.e. unpaired sentiment to sentiment translation. The algorithm is trained to map the source text to the target text, without the target text being available.

## Style Transfer

- [quick overview](https://medium.com/nlplanet/two-minutes-nlp-quick-intro-to-text-style-transfer-61de9cbd4083)

- [Text Style Transfer](https://github.com/fuzhenxin/Style-Transfer-in-Text)
- [Stylized Text Generation Video 1](https://vimeo.com/436479481)
- [Stylized Text Generation Video 2](https://www.youtube.com/watch?v=qSbqVjM-Vik)
