of little circles, for the sake of contrast (“o”).

Click here to view code image

```
plt.plot(A, 2 * A, 'o', A, 1.1 ** A)
```

Alternatively, the two curves could be created by separate statements— with additional spaces inserted here for clarity’s sake.

Click here to view code image

```
plt.plot(A, 2 * A, 'o') # +$2 a year (with circles)
plt.plot(A, 1.1 ** A) # Compound 10% a year
```

Next, let’s specify some useful labels and finally show the graph.

Click here to view code image

```
plt.title('Compounded Interest v. Linear')
plt.xlabel('Years')
plt.ylabel('Value of Funds')
plt.show()
```

Figure 13.10 displays the results. For the first 30 years, the linear fund ($2 a year) outpaces the compound fund. However, between years 30 and 50, the accelerating growth of plan B becomes noticeable. Plan B finally equals and surpasses the linear fund shortly before year 50.