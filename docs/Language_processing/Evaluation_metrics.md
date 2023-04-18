# Evaluation metrics for language processing

When working with language processing, it is important to evaluate the performance of your model. There are a number of metrics that can be used to evaluate the performance of a language processing model. One of the most common methods includes using the [BLEU score](https://en.wikipedia.org/wiki/BLEU). 

## BLEU score

The BLEU score is a metric for evaluating a generated sentence to a reference sentence. The BLEU score ranges from 0 to 1. The closer the score is to 1, the better the generated sentence is. The BLEU score is calculated by comparing n-grams of the generated sentence to the reference sentence.

- [BLEU score introduction using NLTK and Python](https://machinelearningmastery.com/calculate-bleu-score-for-text-python/)

### Corpus BLEU score
Calculate the BLEU score for the following sentences using the NLTK library:

The references must be specified as a list of documents where each document is a list of references and each alternative reference is a list of tokens, e.g. a list of lists of lists of tokens. The candidate documents must be specified as a list where each document is a list of tokens, e.g. a list of lists of tokens.

'''python
from nltk.translate.bleu_score import sentence_bleu
reference = [['this', 'is', 'a', 'test'], ['this', 'is' 'test']]
candidate = ['this', 'is', 'a', 'test']
score = sentence_bleu(reference, candidate)
print(score)
'''
output: 1.0 !perfect match!






