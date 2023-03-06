# Using python for signal processing


## Creating GUI for our application

Creating useful GUI for your aplication can be time consuming. A fast and easy way to create GUI is to use a GUI designer. The guide linked bellow shows how to install and use QT Designer on multiple platforms Windows Mac Linux. QT Designer is a GUI designer that comes with the PyQt5 library. It is a very powerful tool that allows you to create complex GUIs with ease. It also allows you to embed matplotlib plots in your GUI.

- [QT Designer](https://realpython.com/qt-designer-python/#getting-started-with-qt-designer)

Once you desinged your GUI, you can use the `pyuic5` command to convert the `.ui` file to a `.py` file. This file can be imported in your python code.

```pyuic5 -o <output file>.py <input file>.ui```

### Embeding matplotlib plots in QT Designer

- [Embeding matplotlib plots in QT Designer](https://matplotlib.org/stable/gallery/user_interfaces/embedding_in_qt_sgskip.html)

Good tutorial but is not in english. Plenty of nice pictures though.
- [Another similar way](https://yapayzekalabs.blogspot.com/2018/11/pyqt5-gui-qt-designer-matplotlib.html)

First you need to create a empty widget in QT Desiner and promote it to is own class. Then in python you need define a class with the same name inheriting QWidget and a adding FigureCanvas to that widget.

Widget definition in python:

```python

from PyQt5.QtWidgets import*

from matplotlib.backends.backend_qtagg import FigureCanvas

from matplotlib.figure import Figure

    
class MplWidget(QWidget):
    
    def __init__(self, parent = None):

        QWidget.__init__(self, parent)
        
        self.canvas = FigureCanvas(Figure())
        
        vertical_layout = QVBoxLayout()
        vertical_layout.addWidget(self.canvas)
        
        self.canvas.axes = self.canvas.figure.add_subplot(111)
        self.setLayout(vertical_layout)

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
### Ploting realtime data

It similar to plotting normal data but we use a `FunAnimation()` to update the plot with new data

```python
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation
import math

if __name__ == '__main__':

    fig = plt.figure()
    ax = fig.add_subplot(111)

    phase = 0
    data = [0,0,0,0,0,0,0,0,0,0]

    def update_data(args):
        global phase
        #generate new data
        phase += 1
        for i in range(10):
            data[i] = math.sin(phase+i)

        ax.cla() #clear previous data
        ax.plot(data)

    ani = FuncAnimation(fig, update_data, interval=200)
    plt.show()
```

!Note: animation is not supported in the jupyter notebook but plotting data normally works fine.

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

## Alternative using pyROOT to plot data

[PyROOT](https://root.cern/manual/python/) is a python module that allows you to use the ROOT libraries in python. The following example shows how to use pyROOT to plot data. It is able to plot data from numpy arrays and pandas dataframes.

### install pyROOT

There are several ways to install pyROOT. The easiest way is to install it with conda.

[More on ROOT installation](https://root.cern/install/)

### example

```python

import ROOT

if __name__ == '__main__':

    # create a canvas
    canvas = ROOT.TCanvas("canvas", "canvas", 800, 600)

    # create a histogram
    hist = ROOT.TH1F("hist", "hist", 100, 0, 100)

    # fill the histogram
    hist.FillRandom("gaus", 1000)

    # draw the histogram
    hist.Draw()

    # show the plot
    canvas.Draw()

```