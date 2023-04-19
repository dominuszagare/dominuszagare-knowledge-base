# Working with NLTK, TorchText, Sklearn, and PyTorch

There are a number of libraries that can be used to work with natural language processing. The most common libraries are NLTK, [TorchText](https://github.com/pytorch/text), Sklearn, and PyTorch. In this article, we will go through some examples of how to use these libraries.

## Word vectorization

- [Word vectorization](https://www.analyticsvidhya.com/blog/2021/06/part-5-step-by-step-guide-to-master-nlp-text-vectorization-approaches/)

A neural networks can only work with numbers. To use text as input we need to convert it to numbers. This is called vectorization. There are several ways to vectorize text. The most common are: count vectorizer, bag of words and n-grams. In all cases the text is first tokenized:
- [TorchText tokenizers](https://pytorch.org/text/stable/transforms.html#module-torchtext.transforms)


## BLEU score

Many of the libaries have built in functions for calculating the BLEU score. The BLEU score is a metric for evaluating a generated sentence to a reference sentence. The BLEU score ranges from 0 to 1. The closer the score is to 1, the better the generated sentence is. The BLEU score is calculated by comparing n-grams of the generated sentence to the reference sentence.

- [BLUE score torchText](https://pytorch.org/text/stable/data_metrics.html#bleu-score)


## Parallel vs non-parallel corpora / supervised vs unsupervised

Text Style Transfer algorithms ca be developed in a supervised way, using parallel corpora, i.e a corpus of text in the source language and a corpus of text in the target language. The algorithm is trained to map the source text to the target text. For unsupervised style transfer, we can use non-parallel corpora, i.e. unpaired sentiment to sentiment translation. The algorithm is trained to map the source text to the target text, without the target text being available.

## Style Transfer

- [quick overview](https://medium.com/nlplanet/two-minutes-nlp-quick-intro-to-text-style-transfer-61de9cbd4083)

- [Text Style Transfer](https://github.com/fuzhenxin/Style-Transfer-in-Text)
- [Stylized Text Generation Video 1](https://vimeo.com/436479481)
- [Stylized Text Generation Video 2](https://www.youtube.com/watch?v=qSbqVjM-Vik)
