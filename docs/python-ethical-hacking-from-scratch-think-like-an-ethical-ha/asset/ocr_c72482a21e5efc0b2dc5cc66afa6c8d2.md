Let's create a new folder in Kali where all our project files will be present:

+   1. Open your Kali home directory and create a new folder called `python-hacking`. All our future work will be done here.

+   2. Open this folder in VS Code:

![](bbox=[0.355, 0.191, 0.612, 0.31])

Figure 2.15 – Folder structure inside VS Code

+   3. Inside the `python-hacking` folder, create a new folder called `m1-hello-world`. Here we will test our virtual environment. Inside the `m1-hello-world` folder, create a new file called `main.py`.

+   4. Check whether the Python package manager, `pip`, is installed properly in Kali using the following command in terminal:

```
pip3 --version
```

You should see the following output:

![](bbox=[0.345, 0.508, 0.622, 0.56])

Figure 2.16 – pip not installed

+   5. If you see an output similar to the preceding one, it means that `pip` is not installed on the system. To install `pip`, run the following command. Make sure that the system is updated:

```
sudo apt install python3-pip
```

Once pip is installed properly, the output will be like this:

![](bbox=[0.165, 0.718, 0.797, 0.777])

Figure 2.17 – pip installation