You have seen that the styling is applied globally to the entire notebook, and you have learned to switch to the default styling. You can locally change the styling for a block of code as follows:

```
with plt.style.context('Solarize_Light2'):
    data = np.linspace(0, 6 * np.pi)
    plt.plot(np.sin(data), 'g--')
    plt.show()
```

Figure 5-7 shows the output.

![](bbox=[0.23, 0.33, 0.78, 0.6])

Figure 5-7. Temporary styling

# Layouts

In this section, you will study layouts. You already learned about subplots in Chapter 4, and if you want to use the default style again, you can run the following line of code to reset the styling to the default style:

```
plt.style.use('default')
```