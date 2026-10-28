The `write_html()` method requires one argument: the name of the file to write to. If you only provide a filename, the file will be saved in the same directory as the `.py` file. You can also call `write_html()` with a `Path` object, and write the output file anywhere you want on your system.

# TRY IT YOURSELF

**15-6. Two D8s:** Create a simulation showing what happens when you roll two eight-sided dice 1,000 times. Try to picture what you think the visualization will look like before you run the simulation, then see if your intuition was correct. Gradually increase the number of rolls until you start to see the limits of your system’s capabilities.

**15-7. Three Dice:** When you roll three D6 dice, the smallest number you can roll is 3 and the largest number is 18. Create a visualization that shows what happens when you roll three D6 dice.

**15-8. Multiplication:** When you roll two dice, you usually add the two numbers together to get the result. Create a visualization that shows what happens if you multiply these numbers by each other instead.

**15-9. Die Comprehensions:** For clarity, the listings in this section use the long form of `for` loops. If you’re comfortable using list comprehensions, try writing a comprehension for one or both of the loops in each of these programs.

**15-10. Practicing with Both Libraries:** Try using Matplotlib to make a die-rolling visualization, and use Plotly to make the visualization for a random walk. (You’ll need to consult the documentation for each library to complete this exercise.)

# Summary

In this chapter, you learned to generate datasets and create visualizations of that data. You created simple plots with Matplotlib and used a scatter plot to explore random walks. You also created a histogram with Plotly, and used it to explore the results of rolling dice of different sizes.

Generating your own datasets with code is an interesting and powerful way to model and explore a wide variety of