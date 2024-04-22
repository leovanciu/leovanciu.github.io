---
title: "Computational Performance Comparison"
output:
  md_document:
    variant: markdown_github
    preserve_yaml: true
always_allow_html: true
---

# Introduction

This project consists of a comprehensive comparison of execution time
and memory use in Python, R for computational operations common in data
science, namely generic loop and vectorized operations, matrix
multiplication and inversion, and some popular computationally heavy
statistical and ML algorithms.

## Why R and Python

R and Python are arguably the two most popular open-source programming
language used in data science. Python is also widely outside for
programming outside data science and ranks first in many popularity
indices like the TIOBE index. Its popularity in data science is mainly
due to its extensive ecosystem of libraries contributed by the wide
programming community, such as NumPy, pandas, and SciPy, which provide
data manipulation, analysis, and visualization. Python’s ecosystem also
has libraries beyond data science, such as for web development like
Django and Flask and natural language processing like NLTK and spaCy.
Python has a user-friendly syntax and extensive documentation on GitHub
and is particularly popular in the data science industry and in the
academic machine learning community. Python libraries are typically
built in C or C++ and provide efficient implementations of most popular
data science algorithms while keeping the code simple and readable.

R is a programming language designed specifically for statistical
computing and graphics. Likewise to Python, R’s main strength is its
extensive collection of packages contributed by the community of users,
which mainly consists of statisticians and researchers. It features
built-in functions for popular statisical algorithms like linear
regression as well as graphical capabilities, and there are many simple
and high-quality visualization tools through packages like ggplot2 and
plotly. R’s syntax is tailored for statistical analysis, using vectors
as its data structure for storing and manipulating data and featuring
built-in reproducibility features. Its packages are also typically
implemented in C or C++.

Other popular high-level programming languages in data science include
Julia and Java and common proprietary programming languages include SAS,
Stata, and MATLAB.

## Setup

For each algorithm, I measure execution time and memory use using the
benchmark package in R and time and profile libraries in Python. The
algorithms were meant to be written in a way that is as structurally
similar as possible, but since both R and Python are high level
languages the implementations of base functions vary greatly between the
two languages. Ultimately, this is what makes the comparison
interesting, but it also means that an algorithm can be poorly optimized
in a language and seem much faster in the other. For instance, people
often criticize the speed of loops in R and my tests agree with this
criticism, but these operations can almost always be made much faster by
using vectorized operations.

I simulated datasets in R typically from a standard Normal distribution,
with sample sizes varying 10<sup>3</sup> to 10<sup>7</sup>. I ran the
scripts in R and Python on these simulated datasets and their execution
time and memory use were recorded in CSV files. I used R markdown to
generate this webpage, using the plotly package to generate interactive
plots. I also included theoretical Big-O complexity of the algorithms
based on the most simple form of the algorithm.

The following algorithms were tested: a simple loop and a vectorized
implementation, matrix multiplication and inversion, linear regression,
a bootstrap algorithm, and a SVM algorithm. When possible, I have
implemented a simple version of these algorithms using only base
functions in the language as well as a popular package/library. For
instance, for linear regression I use the lm() function in R and the
Scikit-Learn library in Python as well as an algorithm using only matrix
multiplication and inversion. Of course, these two matrix operations are
themselves algorithms with many different possible implementations that
I do not know anything about and which are apparently active areas of
research in computer science. I am somewhat biased since I mainly use R
in my academic work and am more familiar with the most optimized
packages in R rather than Python.

## Loop Operations

### Performance Time Comparison

This is fake data.

``` r
library(plotly)

sample_sizes <- c(100, 1000, 10000, 100000)
python_times <- sample_sizes/100
r_times <- sqrt(sample_sizes)
julia_times <- log(sample_sizes)

data <- data.frame(
  SampleSize = rep(sample_sizes, each = 3),
  Time = c(python_times, r_times, julia_times),
  Language = factor(rep(c("Python", "R", "Julia"), times = 4))
)

# Create an interactive plotly plot
p <- plot_ly(data, x = ~SampleSize, y = ~Time, color = ~Language, type = 'scatter', mode = 'lines+markers')
p
```

<div class="plotly html-widget html-fill-item" id="htmlwidget-752b13970578d3afb81d" style="width:672px;height:480px;"></div>
<script type="application/json" data-for="htmlwidget-752b13970578d3afb81d">{"x":{"visdat":{"110d86b6677dc":["function () ","plotlyVisDat"]},"cur_data":"110d86b6677dc","attrs":{"110d86b6677dc":{"x":{},"y":{},"mode":"lines+markers","color":{},"alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"scatter"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":"SampleSize"},"yaxis":{"domain":[0,1],"automargin":true,"title":"Time"},"hovermode":"closest","showlegend":true},"source":"A","config":{"modeBarButtonsToAdd":["hoverclosest","hovercompare"],"showSendToCloud":false},"data":[{"x":[100,1000,10000,100000],"y":[100,31.622776601683793,4.6051701859880918,11.512925464970229],"mode":"lines+markers","type":"scatter","name":"Julia","marker":{"color":"rgba(102,194,165,1)","line":{"color":"rgba(102,194,165,1)"}},"textfont":{"color":"rgba(102,194,165,1)"},"error_y":{"color":"rgba(102,194,165,1)"},"error_x":{"color":"rgba(102,194,165,1)"},"line":{"color":"rgba(102,194,165,1)"},"xaxis":"x","yaxis":"y","frame":null},{"x":[100,1000,10000,100000],"y":[1,1000,100,6.9077552789821368],"mode":"lines+markers","type":"scatter","name":"Python","marker":{"color":"rgba(252,141,98,1)","line":{"color":"rgba(252,141,98,1)"}},"textfont":{"color":"rgba(252,141,98,1)"},"error_y":{"color":"rgba(252,141,98,1)"},"error_x":{"color":"rgba(252,141,98,1)"},"line":{"color":"rgba(252,141,98,1)"},"xaxis":"x","yaxis":"y","frame":null},{"x":[100,1000,10000,100000],"y":[10,10,316.22776601683796,9.2103403719761836],"mode":"lines+markers","type":"scatter","name":"R","marker":{"color":"rgba(141,160,203,1)","line":{"color":"rgba(141,160,203,1)"}},"textfont":{"color":"rgba(141,160,203,1)"},"error_y":{"color":"rgba(141,160,203,1)"},"error_x":{"color":"rgba(141,160,203,1)"},"line":{"color":"rgba(141,160,203,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.20000000000000001,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
