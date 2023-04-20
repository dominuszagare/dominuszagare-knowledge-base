# Working with NLTK, TorchText, Sklearn, and PyTorch

There are a number of libraries that can be used to work with natural language processing. The most common libraries are NLTK, [TorchText](https://github.com/pytorch/text), Sklearn, and PyTorch. In this article, we will go through some examples of how to use these libraries.

## Word vectorization, tokenization or Word Embedding

- [Word vectorization](https://www.analyticsvidhya.com/blog/2021/06/part-5-step-by-step-guide-to-master-nlp-text-vectorization-approaches/)

A neural networks can only work with numbers. To use text as input we need to convert it to numbers. This is called vectorization. There are several ways to vectorize text. The most common are: count vectorizer, bag of words and n-grams. In all cases the text is first tokenized:
- [TorchText tokenizers](https://pytorch.org/text/stable/transforms.html#module-torchtext.transforms)
- [NLTK tokenizers](https://www.nltk.org/api/nltk.tokenize.html)

## BLEU score

Many of the libaries have built in functions for calculating the BLEU score. The BLEU score is a metric for evaluating a generated sentence to a reference sentence. The BLEU score ranges from 0 to 1. The closer the score is to 1, the better the generated sentence is. The BLEU score is calculated by comparing n-grams of the generated sentence to the reference sentence.

- [BLUE score torchText](https://pytorch.org/text/stable/data_metrics.html#bleu-score)


## Parallel vs non-parallel corpora / supervised vs unsupervised

Text Style Transfer algorithms ca be developed in a supervised way, using parallel corpora, i.e a corpus of text in the source language and a corpus of text in the target language. The algorithm is trained to map the source text to the target text. For unsupervised style transfer, we can use non-parallel corpora, i.e. unpaired sentiment to sentiment translation. The algorithm is trained to map the source text to the target text, without the target text being available.

## Non-sequencial data vs sequencial data

In non-sequential data we don't really care from where the data is generated, it can be from a csv, a database, a file, etc. Usually non-sequential data can be described by some distribution and we don't really care in wat order it is presented to the model. In sequential data we care about the order of the data. For example, in a text generation model, the order of the words is important. In a text classification model, the order of the words is not important.


## Teacher forcing

- [Teacher forcing](https://machinelearningmastery.com/teacher-forcing-for-recurrent-neural-networks/)
- [Teacher forcing](https://www.youtube.com/watch?v=vQ9_4tlYXSA&ab_channel=RANJIRAJ/)

Teacher forcing is a strategy for training recurrent neural networks that uses ground truth as input, instead of model output from a prior time step as an input.

Imagine we want to train a model to generate the next word in the sequence given the previous sequence of words. We can use the previous sequence of words as input to the model. However, this is not the best way to train the model. The best way to train the model is to use the ground truth as input. This is called teacher forcing. Example: `[START] Mary had a little lamb whose fleece was white as snow [END]` Imagine the model generates the word “a“, but of course, we expected “Mary“. We can discard this output after calculating error and feed in “Mary” as part of the input on the subsequent time step. We can then repeat this process for each input-output pair of words.
```
[START], ?
[START], Mary, ?
[START], Mary, had, ?
[START], Mary, had, a,	?
```

## Pre-trained models

Traning language models from scratch is a very time consuming process. It is much faster to use a pre-trained model. There are a number of pre-trained models available. The most common are:
- [GPT-2](https://openai.com/blog/better-language-models/)
- [BERT](https://huggingface.co/transformers/model_doc/bert.html)
- [GPT-3](https://openai.com/blog/gpt-3-apps/)
- [XLNet](https://huggingface.co/transformers/model_doc/xlnet.html)
- [RoBERTa](https://huggingface.co/transformers/model_doc/roberta.html)
- [DistilBERT](https://huggingface.co/transformers/model_doc/distilbert.html)
- [ALBERT](https://huggingface.co/transformers/model_doc/albert.html)
- [T5](https://huggingface.co/transformers/model_doc/t5.html)
- [BART](https://huggingface.co/transformers/model_doc/bart.html)
- [GPT Neo](https://huggingface.co/transformers/model_doc/gpt_neo.html)
- [Longformer](https://huggingface.co/transformers/model_doc/longformer.html)
- [Reformer](https://huggingface.co/transformers/model_doc/reformer.html)
- [LayoutLM](https://huggingface.co/transformers/model_doc/layoutlm.html)

The selection of pre-trained models is growing rapidly. The pre-trained models can be used for a number of tasks, such as:
- [Text classification](https://huggingface.co/transformers/task_summary.html#text-classification)
- [Text generation](https://huggingface.co/transformers/task_summary.html#text-generation)
- [Question answering](https://huggingface.co/transformers/task_summary.html#question-answering)
- [Summarization](https://huggingface.co/transformers/task_summary.html#summarization)
- [Translation](https://huggingface.co/transformers/task_summary.html#translation)
- [Feature extraction](https://huggingface.co/transformers/task_summary.html#feature-extraction)
- [Named entity recognition](https://huggingface.co/transformers/task_summary.html#named-entity-recognition)
- [Masked language modeling](https://huggingface.co/transformers/task_summary.html#masked-language-modeling)
- [Multiple choice](https://huggingface.co/transformers/task_summary.html#multiple-choice)
- [Conversational](https://huggingface.co/transformers/task_summary.html#conversational)
- [Language modeling](https://huggingface.co/transformers/task_summary.html#language-modeling)
- [Sentiment analysis](https://huggingface.co/transformers/task_summary.html#sentiment-analysis)
- [Token classification](https://huggingface.co/transformers/task_summary.html#token-classification)


## Style Transfer

- [quick overview](https://medium.com/nlplanet/two-minutes-nlp-quick-intro-to-text-style-transfer-61de9cbd4083)

- [Text Style Transfer](https://github.com/fuzhenxin/Style-Transfer-in-Text)
- [Stylized Text Generation Video 1](https://vimeo.com/436479481)
- [Stylized Text Generation Video 2](https://www.youtube.com/watch?v=qSbqVjM-Vik)
