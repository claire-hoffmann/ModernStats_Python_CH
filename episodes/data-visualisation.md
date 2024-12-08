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
