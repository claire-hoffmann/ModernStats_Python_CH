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

## Quick and appealing plots with Seaborn

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

Seaborn includes several built-in datasets, such as `tips`, `iris`, and `flights`, that we will use throughout this lesson for our examples. These datasets are great for experimenting with Seaborn’s plotting functions without needing to import external data files.

### Creating Seaborn plots

With Seaborn too, the `plt.subplots()` function is used to create a figure (`fig`) and one or more axes (`ax`) that can be used to draw plots.

```python
import seaborn as sns
import matplotlib.pyplot as plt

# Create the figure and axes
fig, ax = plt.subplots()

# Use Seaborn to create a plot on the axes
sns.set(style="whitegrid")
data = sns.load_dataset("tips")
sns.boxplot(x="day", y="total_bill", data=data, ax=ax)

# Customize with Matplotlib
ax.set_title("Total Bill by Day")
ax.set_xlabel("Day of the Week")
ax.set_ylabel("Total Bill ($)")

# Show the plot
plt.show()
```

In this example, we create a boxplot using Seaborn, but we specify the axes (`ax`) created with `plt.subplots()`. This allows us to use Matplotlib to customize the plot's title, labels, and size.

The other types of graphs available in Seaborn include:

- Distribution plots

```python
fig, ax = plt.subplots(figsize=(8, 6))
sns.histplot(data["total_bill"], ax=ax)
ax.set_title("Histogram of Total Bill")
plt.show()
```

- Scatter plots

```python
fig, ax = plt.subplots(figsize=(8, 6))
sns.scatterplot(x="total_bill", y="tip", data=data, ax=ax)
ax.set_title("Total Bill vs Tip")
plt.show()
```

Seaborn's `scatterplot()` function allows you to add additional features like color and size based on other variables.

- Line plots

```python
fig, ax = plt.subplots(figsize=(8, 6))
sns.lineplot(x="size", y="total_bill", data=data, ax=ax)
ax.set_title("Total Bill vs Size of Party")
plt.show()
```

### Customizing Seaborn plots

One of the benefits of using Seaborn is its simplicity and attractive default themes, but you can still customize the appearance of plots using Matplotlib functions. For example, you can set the plot's title, labels, or customize the grid.

```python
# Customizing plot appearance
fig, ax = plt.subplots(figsize=(8, 6))
sns.boxplot(x="day", y="total_bill", data=data, ax=ax)
ax.set_title("Total Bill by Day")
ax.set_xlabel("Day of the Week")
ax.set_ylabel("Total Bill ($)")
ax.grid(True)  # Add gridlines
plt.show()
```

## Interactive plots with Plotly

Plotly is an interactive graphing library that enables the creation of sophisticated visualizations that are interactive by default. Unlike static libraries like Matplotlib and Seaborn, Plotly allows you to zoom, pan, and hover over data points to inspect values directly in the plot.

Advantages of Plotly include:

- Interactive Plots: Plots in Plotly are interactive out of the box, making them ideal for exploring data.
- Web Integration: Plotly graphs can easily be integrated into web applications, such as Dash.
- High-quality Visualizations: Plotly can generate a wide range of high-quality, aesthetically appealing plots.

### Importing `plotly`

Plotly’s most commonly used module for creating visualizations is plotly.express. Here’s how to import it:

```python
import plotly.express as px
```

`plotly.graph_objects` is another module in Plotly that provides more flexibility for creating complex visualizations. However, we will primarily focus on plotly.express as it simplifies the syntax for most common plots.

### Creating Simple Plots with Plotly Express

#### Line Plot

```python
import pandas as pd

# Sample data

data = pd.DataFrame({
"Date": pd.date_range(start="2024-01-01", periods=10, freq="D"),
"Value": [10, 12, 13, 15, 16, 18, 19, 20, 21, 22]
})

# Create a line plot

fig = px.line(data, x="Date", y="Value", title="Simple Line Plot")
fig.write_html("plot.html")
```

In this code:

- `px.line()` creates the line plot.
- The x and y arguments specify which columns to plot.
- `fig.write_html()` saved the plot as a HTML file.

#### Scatter Plot

```python
import numpy as np

# Sample data

data = pd.DataFrame({
"X": np.random.rand(100),
"Y": np.random.rand(100)
})

# Create a scatter plot

fig = px.scatter(data, x="X", y="Y", title="Scatter Plot")
fig.write_html("plot.html")
```

#### Bar Chart

```python
categories = ['A', 'B', 'C', 'D', 'E']
values = [23, 45, 56, 78, 33]
data = pd.DataFrame({"Category": categories, "Value": values})

# Create a bar chart

fig = px.bar(data, x="Category", y="Value", title="Bar Chart")
fig.write_html("plot.html")
```

#### Pie Chart

```python
# Sample data for Pie Chart

categories = ['Red', 'Blue', 'Green']
values = [25, 50, 25]
data = pd.DataFrame({"Category": categories, "Value": values})

# Create a pie chart

fig = px.pie(data, names="Category", values="Value", title="Pie Chart")
fig.write_html("plot.html")
```

### Customizing Plots in Plotly Express

Plotly Express automatically makes plots interactive, but you can also customize your plots to make them more informative and visually appealing.

#### Adding Titles and Labels

You can modify the title and axis labels of your plot:

```python
fig.update_layout(
title="Updated Plot Title",
xaxis_title="Custom X Axis Label",
yaxis_title="Custom Y Axis Label"
)
fig.write_html("plot.html")
```

#### Changing Colors

You can change the color of data points or bars based on a categorical variable:

```python
# Adding a color dimension

data['Color'] = np.random.choice(['Red', 'Blue', 'Green'], size=100)

fig = px.scatter(data, x="X", y="Y", color="Color", title="Colored Scatter Plot")
fig.write_html("plot.html")
```

In this example, the color argument differentiates data points by color based on the `Color` column.

### Plotly Express vs. Plotly Graph Objects

While `plotly.express` is great for creating quick, simple plots, there are cases when you might need more control over the plot’s components. This is where `plotly.graph_objects` comes in.

Plotly Graph Objects (`go`) is a lower-level interface that gives you finer control over the layout and elements of your plot. With go, you can manually define traces (such as lines, bars, and scatter plots), customize plot attributes, and handle more complex visualizations.

When to use `plotly.graph_objects`:

- **Multiple Traces**: When you need to add different types of plots (like a line and scatter plot) in the same figure.
- **Advanced Customization**: For precise control over each plot element (e.g., customizing legends, adding annotations).
- **Complex Layouts**: When you need subplots or advanced arrangements of figures.

For example, if you wanted to combine a line and scatter plot on the same figure, you would use `plotly.graph_objects`:

```python
import plotly.graph_objects as go

# Create a figure with both a scatter and line trace

fig = go.Figure()

# Scatter plot trace

fig.add_trace(go.Scatter(x=data["X"], y=data["Y"], mode='markers', name="Scatter"))

# Line plot trace

fig.add_trace(go.Scatter(x=data["X"], y=data["Y"], mode='lines', name="Line"))

# Save the plot

fig.write_html("plot.html")
```

While `plotly.express` handles this type of task easily with fewer lines of code, `go` offers more flexibility for complex customizations.

### Interactive Features of Plotly Express

Plotly plots are interactive by default. These features include:

- **Zooming and Panning**: Users can zoom into a region of the plot by dragging the mouse, and pan across it.
- **Hovering**: When you hover over data points, Plotly shows additional information (e.g., exact values).
- **Saving and Exporting**: You can save your plot as an image or an interactive HTML file.
