# Signal filtering

Sometimes to make the data useful we must filter out any undesired noise or signals that are not what we are interested in

## Types of filters

There are many types of filters, but the most common are:

- Low-pass filter
- High-pass filter
- Band-pass filter
- Band-stop filter
## Low-pass filter

A low-pass filter is a filter that passes signals with a frequency lower than a certain cutoff frequency and attenuates signals with frequencies higher than the cutoff frequency.

## implementing a low-pass filter in python

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy import signal

if __name__ == '__main__':

    # create a signal
    t = np.linspace(0, 1, 500, endpoint=False)
    sig = np.sin(2 * np.pi * 50 * t) + 0.5 * np.sin(2 * np.pi * 120 * t)

    # create a low pass filter
    b, a = signal.butter(4, 0.05, 'low', analog=True)

    # apply the filter to the signal
    filtered_sig = signal.filtfilt(b, a, sig)

    # plot the signal
    plt.plot(t, sig, label='original signal')

    # plot the filtered signal
    plt.plot(t, filtered_sig, label='filtered signal')

    # show the plot
    plt.legend()
    plt.show()
```
## High-pass filter

A high-pass filter is a filter that passes signals with a frequency higher than a certain cutoff frequency and attenuates signals with frequencies lower than the cutoff frequency.

## Band-pass filter

A band-pass filter is a filter that passes signals within a certain frequency range and attenuates signals with frequencies outside of that range.

## Band-stop filter

A band-stop filter is a filter that attenuates signals within a certain frequency range and passes signals with frequencies outside of that range.
## Links

-[Designing filters](https://www.youtube.com/watch?v=uNNNj9AZisM&t=881s&ab_channel=Phil%E2%80%99sLab)
- [Filter (signal processing)](https://en.wikipedia.org/wiki/Filter_(signal_processing))
- [Butterworth filter](https://en.wikipedia.org/wiki/Butterworth_filter)
- [Chebyshev filter](https://en.wikipedia.org/wiki/Chebyshev_filter)
- [Elliptic filter](https://en.wikipedia.org/wiki/Elliptic_filter)
- [Filter design](https://en.wikipedia.org/wiki/Filter_design)
- [Filter design in Python](https://docs.scipy.org/doc/scipy/reference/signal.html#filter-design)


