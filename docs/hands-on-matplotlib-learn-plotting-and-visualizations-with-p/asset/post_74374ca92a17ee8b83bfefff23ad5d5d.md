
## CHAPTER 1  INTRODUCTION TO PYTHON 3

Server stopped

You have shut down Jupyter. You can now close this tab.
To use Jupyter again, you will need to relaunch it.

![](img/124f805c8155c7226d129ebbcc2842f0_36_0.png)

To work with the Jupyter Notebook, you need to execute the command jupyter notebook again at the command prompt.

On the top-right side, just below the Quit and Logout buttons, you can see a small button with a refresh symbol. This button refreshes the home page. You also have the New button. Once clicked, it shows a drop-down, as shown in Figure 1-17.

![](img/124f805c8155c7226d129ebbcc2842f0_36_1.png)

*Figure 1-17. Options for a new notebook*

As you can see, the drop-down is divided into two sections, Notebook and Other. You can create the Octave and Python 3 notebooks. If your computer has more programming languages installed that are supported by Jupyter Notebook, then all those languages will show up here. You can also create text files and folders. You can open a command prompt in the web browser by clicking Terminal. Figure 1-18 shows terminal running in a separate web browser tab.

![](img/124f805c8155c7226d129ebbcc2842f0_37_0.png)

*Figure 1-18.* A new terminal window within the browser

Clicking Python 3 in the drop-down creates a new Python 3 notebook, as shown in Figure 1-19.

![](img/124f805c8155c7226d129ebbcc2842f0_37_1.png)

*Figure 1-19.* A new Python 3 notebook

If you go to the home page again by clicking the home page tab in the browser and then open the Running tab in the home page, you can see the entries corresponding to the terminal and the Python 3 notebook, as shown in Figure 1-20.

![](img/124f805c8155c7226d129ebbcc2842f0_38_0.png)

Figure 1-20. Summary of current Jupyter Notebook subprocesses

## Running Code in Jupyter Notebook

Go to Python 3’s Untitled1 tab again and type in the following statement in the text area (also known as a cell):

```
printf("Hello, World!\n");
```

Then click the Run button. Jupyter will execute the statement as a Python 3 statement and show the result immediately below the cell, as shown in Figure 1-21.

![](img/124f805c8155c7226d129ebbcc2842f0_38_1.png)

Figure 1-21. Code output in Jupyter Notebook

As you can see, after execution, it automatically creates a new cell below the result and sets the cursor there. Let’s discuss the menu bar and the icons above the programming cells. You can save the file by clicking the floppy disk icon. You can add a new empty cell after the current cell by clicking the + icon. The next three icons are Cut, Copy, and Paste. Using the up and down arrows can shift the position of the current cell up and down, respectively. The next option is to run the cell, which you already saw. The next three icons are for interrupting the kernel, restarting the kernel, and rerunning all the cells in the notebook. Next to that, you have a drop-down that tells you what type of cell it should be. Figure 1-22 shows the drop-down when clicked.

![](img/124f805c8155c7226d129ebbcc2842f0_39_0.png)

Figure 1-22. Types of cells in Jupyter Notebook

The cell is treated as a Python 3 code cell when you choose the Code option. It is treated as a Markdown cell when you choose the Markdown option. Markdown is a markup language that can create rich-text output. For example, anything followed by # creates a heading, anything followed by ## creates a subheading, and so on. Just type the following lines in a Markdown cell and execute them:

```
# Heading 1
## Heading 2
```

During our Python 3 demonstrations, we will mostly use Markdown for headings. However, you can further explore Markdown on your own by visiting https://jupyter-notebook.readthedocs.io/en/stable/examples/Notebook/Working%20With%20Markdown%20Cells.html. Figure 1-23 shows the output of the previous demonstration.

You can clear the output of a cell or the entire notebook. In the menu bar, click the Cell menu. In the drop-down, Current Outputs and All Output have a Clear option that clears the output of cells. Figure 1-25 shows the options.

![](img/124f805c8155c7226d129ebbcc2842f0_41_0.png)

Figure 1-25. Clearing the output in Jupyter

One of the most significant advantages of Jupyter Notebook is that you can edit an already executed cell if there is any syntax error or you simply want to change the code. Jupyter Notebook is like an IDE that runs within a web browser and produces the output in the same window. This interactivity and facility to keep code, rich text, and output in the same file has made Jupyter Notebook project hugely popular worldwide. The kernel for running Python programs comes from the IPython project. As I mentioned earlier, you can use it for other programming languages too. I have used it for running GNU Octave programs.

You can find more information about Jupyter Notebook and IPython at the following URLs:

```
https://jupyter.org/
https://ipython.org/
```

## Anaconda

Before we can conclude the chapter, we need to discuss the Python distributions. A Python distribution is nothing more than the Python interpreter bundled with Python libraries. One such popular distribution is Anaconda. You can download and install Anaconda on Linux, Windows, and macOS. Anaconda has many versions. One of them is free and meant for individual usage. You can find it at https://www.anaconda.com/products/individual.

Anaconda comes with an open source package manager that can install packages for Python and other programs. It is known as Conda. You can find more information about the Conda package manager at https://docs.conda.io/en/latest/.

If you have already installed Python from Python’s website, I recommend using another computer to install Anaconda. Having multiple interpreters and distributions of Python can be confusing.

## Summary

In this chapter, you learned the basics of Python programming language. You learned how to write basic Python programs and how to execute them in various ways. You learned to work with Python on various operating systems such as Windows and Linux. You also learned various modes of the Python programming language and how to launch Python from the command prompts of various operating systems. You learned the basics of the built-in package manager of Python, called pip. We also briefly discussed other IDEs for Python.

Then, you got a brief introduction to the scientific Python ecosystem. We will explore many components of this ecosystem in the coming chapters. You also learned how to install Jupyter Notebook on various platforms and explored how you can run simple Python statements in Jupyter Notebook. You learned that you can store the code and the output of the same code in a single file that can be shared easily over the Internet and other media such as portable storage devices.

In the next chapter, we will get started with NumPy.

## CHAPTER 2

## Getting Started with NumPy

In the previous chapter, you learned the basics of the Python programming language and the scientific Python ecosystem. You also learned how to run simple Python programs in interactive web-based notebooks with Jupyter. We will continue using Jupyter Notebook for the majority of demonstrations in the rest of the chapters in the book.

In this chapter, we will give a brief overview of the NumPy library with a few coding demonstrations. The following is the list of topics that we will explore in this chapter:

- Introduction to the NumPy Ndarrays
- Ndarray properties
- NumPy constants

Throughout the remaining chapters of this book, we will explore many components of the scientific Python ecosystem one by one. Throughout this book, we will be using different libraries that are part of this scientific Python ecosystem. The valuable knowledge you will gain in this chapter serves as a foundation for the rest of the chapters. As this is an introductory chapter for a broad ecosystem, I have kept it short yet practical.

## NumPy and Ndarrays

NumPy is the fundamental package for numerical computation in Python. We can use it for numerical computations. The most useful feature of the NumPy library is the multidimensional container data structure known as an Ndarray.

An Ndarray is a multidimensional array (also known as a container) of items that have the same datatype and size. We can define the size and datatype of the items at the time of creating the Ndarray. Just like other data structures such as lists, we can access the contents of an Ndarray with an index. The index in an Ndarray starts at 0 (just like arrays in C or lists in Python). We can use Ndarrays for a variety of computations. All the
