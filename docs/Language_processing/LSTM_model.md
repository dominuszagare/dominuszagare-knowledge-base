# LSTM encoder-decoder model

- [RNN explained](https://www.youtube.com/watch?v=Y2wfIKQyd1I&ab_channel=codebasics)
- [LSTM explanation](https://www.youtube.com/watch?v=LfnrRPFhkuY&ab_channel=codebasics)

Long short-term memory or LSTM is a type of recurrent neural network (RNN) that is used for sequence prediction problems. It is able to remember information for a long period of time. It is composed of a cell, an input gate, an output gate and a forget gate. The cell state is the information that is passed from one time step to the next. The input gate decides what information is going to be added to the cell state. The output gate decides what information is going to be output. The forget gate decides what information is going to be forgotten from the cell state. The cell state is updated by the following formula:

$$
\begin{align}
\text{cell state} &= \text{forget gate} \odot \text{cell state} + \text{input gate} \odot \text{input} \\
\text{output} &= \text{output gate} \odot \text{activation function}(\text{cell state})
\end{align}
$$

where $\odot$ is the element-wise multiplication.

## Example predicting random numbers

We can take a sequence of random numbers and try to predict the next number in the sequence. A great example can be found [here](https://machinelearningmastery.com/how-to-develop-lstm-models-for-time-series-forecasting/)

## Example predicting text

Predicting text is a bit more complicated. We need to convert the text into a sequence of numbers. We can do this by creating a dictionary of all the unique characters in the text and then converting each character into a number. We can then use this sequence of numbers as input to the LSTM model. The output of the model will be a sequence of numbers. We can then convert this sequence of numbers back into text by using the dictionary we created earlier.


