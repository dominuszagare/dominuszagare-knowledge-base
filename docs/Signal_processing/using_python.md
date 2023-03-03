# Using python for signal processing

## Multithreading

python can often be slow when processing large amounts of data. To speed up the processing, you can use the `multiprocessing` module. This module allows you to run multiple processes in parallel. The following example shows how to use this module to speed up the processing of a large amount of data. It also useful to use multithreading to implement responsive GUI controls to control the processes in your program.

```python

from threading import Thread
from threading import Lock

# create a shared lock
lock = Lock()

# creating a class that inherits from Thread class
class MyThread(Thread):

    def __init__(self, name, data):

        Super().__init__(self)

        self.name = name

        self.data = data

    def run(self):

        print("Starting " + self.name)

        # do something with data
        # when reading or writing data its good practice to use locks to prevent data corruption when entering and exiting the critical section

        with lock:

            # do something with data

    #...

if __name__ == '__main__':

    #create a new thread and run it
    thread = MyThread("thread1", data)
    thread.start()

```

## Ploting data with matplotlib

Matplotlib is a python library for plotting data. it goes well with numpy and pandas. The following example shows how to plot data with matplotlib.

```python

import matplotlib.pyplot as plt

if __name__ == '__main__':

    # create a figure
    fig = plt.figure()

    # create a subplot
    ax = fig.add_subplot(111)

    # create data
    x = [1, 2, 3, 4, 5]
    y = [1, 4, 9, 16, 25]

    # plot data
    ax.plot(x, y)

    # show the plot
    plt.show()

```
