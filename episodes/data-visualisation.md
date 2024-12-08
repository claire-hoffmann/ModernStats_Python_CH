---
title: Visualizations
teaching: 60
exercises: 60
---

::::::::::::::::::::::::::::::::::::::: objectives

- create graphs and other visualizations using tabular data
- group plots together to make comparative visualizations

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I visualize tabular data in Python?
- How can I group several plots together?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

In this lesson, we will explore how to create visualizations of your data using three popular Python libraries:

- **Matplotlib** is a foundational library for creating static visualizations in Python. It provides a wide range of charts, such as line plots, bar charts, scatter plots, histograms, and more. While it offers great flexibility, it requires more code for customization, making it best suited for basic to moderately complex visualizations.

- **Seaborn** builds on top of Matplotlib and provides a high-level interface for creating attractive and informative statistical graphics. Seaborn comes with several built-in themes and color palettes, and it simplifies many common tasks, such as visualizing data distributions and relationships between variables. It's especially powerful when working with Pandas DataFrames and when creating plots like boxplots, heatmaps, and pair plots.

- **Plotly** is a modern, interactive graphing library that allows you to create beautiful and interactive web-based visualizations. It is designed for creating visualizations that allow users to zoom, hover, and interact with the chart dynamically. Plotly is particularly useful for creating dashboards, 3D plots, and other interactive visualizations that engage users in exploring the data.

These three libraries can be imported using the following aliases:

```python
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px
```

## Basic plots with `matplotlib`

### Creating a figure and axes

In `matplotlib`, the `plt.subplots()` function is a common way to create a figure (`fig`) and axes (`ax`) objects that you can work with to create and customize your plots.

What are Figures and Axes?

- Figure (`fig`):
  - The figure is the entire container for your plot. It holds everything—axes, titles, labels, and any other elements of the plot.
  - You can have multiple figures in a single Python session, and each figure can hold multiple subplots or axes.
- Axes (`ax`):
  - The axes is where your actual plot will appear. It’s a region of the figure that holds the graph. Each axes object has methods for plotting, adding titles, modifying labels, and other customizations.
  - An axes can represent various types of charts, like line plots, bar charts, histograms, etc. Each plot you create will be associated with one axes.

::::::::::::::::::::::::::::::::::::::::: callout

In `matplotlib`, there are two main ways to create plots:

- The state-based interface using `plt.plot()` and similar functions. This is simpler and often fine for quick plots.

```python
# Basic line plot
plt.plot([1, 2, 3], [1, 4, 9])
plt.show()
```

- The object-oriented approach using `fig, ax = plt.subplots()`, which is recommended for more control and flexibility.

```python
# Create a figure and axis
fig, ax = plt.subplots()

# Plot on the axis
ax.plot([1, 2, 3], [1, 4, 9])

# Show the plot
plt.show()
```

When you use `plt.subplots()`, you get access to the figure and axes objects, allowing you to customize everything from the title, labels, grid, axis limits, and more, in a very controlled manner. This is the approach we will use in this episode.

::::::::::::::::::::::::::::::::::::::::::::::::::

### Customizing Plots

In this example, we change the line style and color, add a title, axis labels and legend:

```python
fig, ax = plt.subplots()

# Plotting data
ax.plot([1, 2, 3], [1, 4, 9], linestyle='--', color='r', label="y = x^2")

# Adding title and labels
ax.set_title('Simple Line Plot')
ax.set_xlabel('X Axis')
ax.set_ylabel('Y Axis')

# Adding a legend
ax.legend()

# Show the plot
plt.show()
```

### Types of plot

On top of the line plot that we already created using `ax.plot()`, matplotlib offers many other types of plots, including:

- Bar plot

```python
# Data
categories = ['A', 'B', 'C', 'D']
values = [3, 7, 5, 2]

# Bar plot
fig, ax = plt.subplots()
ax.bar(categories, values)

plt.show()
```

- Horizontal bar plot

```python
# Horizontal bar plot
fig, ax = plt.subplots()
ax.barh(categories, values)

plt.show()
```

- Stacked bar plot

```python
# Data
categories = ['A', 'B', 'C']
values1 = [3, 7, 5]
values2 = [2, 5, 6]

# Stacked bar plot
fig, ax = plt.subplots()
ax.bar(categories, values1, label='Category 1')
ax.bar(categories, values2, bottom=values1, label='Category 2')

ax.legend()

plt.show()
```

- Scatter plot

```python
# Data
x = [1, 2, 3, 4, 5]
y = [1, 4, 9, 16, 25]

# Scatter plot
fig, ax = plt.subplots()
ax.scatter(x, y)

plt.show()
```

- Pie charts

```python
# Data
sizes = [10, 20, 30, 40]
labels = ['A', 'B', 'C', 'D']

# Pie chart
fig, ax = plt.subplots()
ax.pie(sizes, labels=labels)

plt.show()
```

### Multiple Subplots

Using `fig, ax = plt.subplots()` allows you to create multiple subplots within a single figure.

```python
fig, ax = plt.subplots(2, 1)

# Plotting in the first subplot
ax[0].plot([1, 2, 3], [1, 4, 9])
ax[0].set_title('First Plot')

# Plotting in the second subplot
ax[1].bar(['A', 'B', 'C'], [3, 7, 5])
ax[1].set_title('Second Plot')

plt.show()
```

### Saving plots to files

```python
fig, ax = plt.subplots()

# Plotting
ax.plot([1, 2, 3], [1, 4, 9])

# Saving the plot
fig.savefig('plot.png')
```

Matplotlib supports multiple formats, including PNG, PDF, SVG, and more. Use `fig.savefig('filename.format')`.

# Quick and appealing plots with Seaborn

Both Seaborn and Matplotlib are popular Python libraries for data visualization, but they serve different purposes:

1. High-Level vs. Low-Level

- **Matplotlib** is a low-level library, offering full control over plots but requiring more code.
- **Seaborn** is a high-level library built on top of Matplotlib, offering easier and quicker creation of visually appealing statistical plots.

2. Aesthetics

- **Matplotlib** produces basic plots by default, requiring manual styling for better visuals.
- **Seaborn** comes with default styles, making it easier to create polished plots.

3. Plotting Types

- **Matplotlib** supports a wide range of plots but requires extra work for advanced statistical plots.
- **Seaborn** offers specialized plots for statistics (e.g., violin plots, pair plots) with minimal effort.

4. Integration with Pandas

- **Matplotlib** doesn’t integrate directly with Pandas DataFrames.
- **Seaborn** integrates smoothly with Pandas, allowing you to pass DataFrame columns directly into plotting functions.

However, Seaborn is built on top of Matplotlib, meaning that it inherits Matplotlib's flexibility, allowing users to make more detailed customizations as needed.
