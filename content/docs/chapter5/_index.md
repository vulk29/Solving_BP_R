---
# Title, summary, and page position.
linktitle: Chapter 5
summary: Question development. EDA
weight: 1
icon: book-reader
icon_pack: fas

# Page metadata.
title: Chapter 5
date: "2018-09-09T00:00:00Z"
type: book  # Do not modify.
---
## 3. Question development

> *“There are no routine statistical questions, only questionable statistical routines.” — Sir David Cox*

To analyze any dataset you're being faced with in a business situation you will need to first understand it, and understanding comes through asking the right questions. In the later parts you will gain tools which will let you find answers. It is of utmost importance to ask the correct questions first. There is no routine that will let us do that with 100% accuracy, but there is a few good resources out there that teach good habits.
  
There are a few very basic functions that let us explore the data and are omitted in many tutorials. The first thing you must do when getting a dataset is to load it into R, most likely using `read.csv()`. Here, I am going to use the built-in `iris` dataset. But for now, I do not know anything about it. 

First, I would like to find out how does it look, by taking a look at the first couple of observations and the variables (columns), so essentially the "top of the table".

```{r}
head(iris)
```

Now, I have my very preliminary overview. It seems as though everything is in place, the variables are not confused with observations and the data looks to be parsed correctly. But what I still do not know is what are these variables and what do the observations represent. For that, I will use the `str()` and `summary()` functions. The first one stands for _structure_ and gives a compact overview on how many observations does the [data frame](https://www.tutorialspoint.com/r/r_data_frames.htm) have, and crucially the number of variables, their full names and type. `summary()` returns a table basic descriptive summaries of the data by variable. See the outputs for the `iris` dataset below.

```{r}
str(iris)
summary(iris)
```

You might also want to find out what's the _class_ of your data, because there are certain operations that are reserved for fx. data frames. To find out use, `class()`.  
Lastly, you might want to get a good old-fashioned look at the table just like you did back in your excel days. To open your data in a new "tab" in RStudio use `View()`. If you want to just look at one of the variables, for example the _Species_ from the `iris` dataset, you can use `View(iris$Species)`. For looking specific observations (rows), for example the second and fiftieth ones in the iris dataset, type in `View(iris[c(2,50),])`. Copy and paste the code below into your RStudio to try it out.
```
View(iris$Species) # open the Species column in a new tab
View(iris[c(2,50),]) # open the 2nd and 50th rows in a new tab
```
## EDA resources

Once again, "Data Science with R" offers a comprehensive explanation of the topic, with examples and exercises to offer. I strongly recommend to take a look at [Chapter 7](https://r4ds.had.co.nz/exploratory-data-analysis.html). The chapter is heavy on data visualisation which has not been covered yet, so a discaimer that not all the code will make sense nor should it. Alternatively, you may familiarize yourself with [Chapter 3](https://r4ds.had.co.nz/data-visualisation.html) and be ahead of the curve.  
The most important takeaway is understanding the two fundaments of EDA, variation and covariation.

If you learn better by watching, here is a video on univariate EDA, that explores the first fundament, variation. I recommend watching the videos in this section with an enhanced speed `1.25` or `1.5`.

<iframe width="560" height="315" src="https://www.youtube.com/embed/ONrGJF_8onw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture; fullscreen" ></iframe>

The second part on EDA from James Dayhuff deals with multivariate analysis, or as Wickham puts it, covariance.

<iframe width="560" height="315" src="https://www.youtube.com/embed/-tTJHRaPXxk" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


