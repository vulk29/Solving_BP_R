---
# Title, summary, and page position.
linktitle: Chapter 6
summary: My data is not normally distributed, now what?
weight: 6
icon: book-reader
icon_pack: fas

# Page metadata.
title: Chapter 6
date: "2018-09-09T00:00:00Z"
type: book  # Do not modify.
---
## Noisy data pt. 1

In this section we take a step back from modeling and analyze the distribution of our data. Before we largely assumed that it's [normally distributed](https://www3.nd.edu/~rwilliam/stats1/x21.pdf), now we are going to investigate whether that assumption was a warranted one. 

### Q-Q plot

One of the best ways to see if some univariate data (one variable/column) matches a certain distribution is drawing up a quantile-quantile plot. [If you'd like to brush up on what a quantile is, you can do so here.](https://www.youtube.com/watch?v=IFKQLDmRK0Y) The Q-Q plots the quantiles of your sample on the y axis and the theoretical quantiles, for example from the normal distribution on the x axis. Then if the plotted quantiles ('circles' on the graph) fall on a linear function that means that the distribution of the variable comes approximately from some theoretical distribution. In this part we're going to focus on checking if our distributions are normal.

Consider the marketing data from the previous sections. Suppose we wanted to check whether the mean of budgets for Newspaper and Facebook channels is the same. To check that on a 95% significance level we want to perform a t-test. One of the assumptions is that the variables are normally distributed. Let's use a Q-Q plot to verify that visually.

```{r}
par(mfrow = c(1,2))
qqnorm(marketing$facebook)
qqline(marketing$facebook, col = 2)
qqnorm(marketing$newspaper)
qqline(marketing$newspaper, col = 2)
```


Based on these plots, we can see that most of the values (quantiles) of the variables do follow a normal distribution but for the lowest and highest. To the naked eye it looks as though the assumption is mostly correct. However, R is a statistical software and we're always talking about tests, so is it possible to test for normality to be _sure_ (on a 95% level, of course)? 

### Testing for normality

Yes indeed, there is a couple of those tests, but the most popular one is called is the Shapiro-Wilk test. In R, we can easily do it using the built-in `stats` package's `shapiro.test` function. I hope apologies to Wilk have been made.

```{r}
shapiro.test(marketing$facebook)
shapiro.test(marketing$newspaper)
```

The results of the test yield that the null hypothesis, which is "the data belongs to a normal distribution", is rejected for both of the variables. Therefore, we technically cannot assume normality based on the Shapiro-Wilk test, even though it looked promising on the QQ-plot. In both of the distributions we can observe what is called "heavy tails", as in more extreme points than we'd expect in a normal distribution. [Read more about interpreting QQ plots here.](https://data.library.virginia.edu/understanding-q-q-plots/) 

Lastly, be wary that there is [an argument for normality testing being 'essentially useless'](https://stats.stackexchange.com/questions/2492/is-normality-testing-essentially-useless), especially for bigger sample sizes. 


## Noisy data pt. 2

In this section we will introduce methods of making our data more normal and some non-parametric tests.

### Transform data for normality

There are several ways to transform data to make it fit the normal distribution better. The easiest, most straightforward ways are _logarithm_, _square-root_ and _reciprocity_. Incidentally, these are also the most wide-spread. [Read more about 'simple' transformations here.](https://fmwww.bc.edu/repec/bocode/t/transint.html)

It's easy to implement those transformations in R, here we will also visualise a QQ plot to try and assess whether we've improved the distribution of `facebook` from previous examples.

```{r}
par(mfrow = c(1,3))
qqnorm(log10(marketing$facebook+1), main = "Q-Q (log transformation)")
qqline(log10(marketing$facebook), col = 2)
qqnorm(sqrt(marketing$facebook), main = "Q-Q (square-root transformation)")
qqline(sqrt(marketing$facebook), col = 2)
qqnorm(1/(marketing$facebook+1), main = "Q-Q (reciprocity transformation)")
qqline(1/(marketing$facebook+1), col = 2)
```

As we can see on the QQ plots, neither of the transformations improved the distribution's likeness to the Gaussian curve. Therefore, we might look to some more complex transformations such as the _Box-Cox_ transformation, _Yeo-Johnson_ and more. Those methods are much more difficult to interpret and implement. I recommend reading [this CRAN vignette](https://cran.r-project.org/web/packages/bestNormalize/vignettes/bestNormalize.html) about the `bestNormalize` package designed for picking the best method and using the methods with the `MASS` package. 

### Non-parametric methods

Another approach to having a non-normally distributed data is changint the statistical method of testing rather than trying to "normalize" the data which clearly isn't. Such statistical methods are called _non-parametric_, because they do _not_ estimate the parameters assuming a distribution. 

Some of the most popular tests you might come across are: Mann-Whitney U, Wilcoxon Signed Rank, Kruskal Wallis, and Friedman tests. [For an R guideline, click here.](https://www.statmethods.net/stats/nonparametric.html)

In our example, what we wanted to do in the first place was compare the means of two numeric variables, `facebook` and `newspaper` using a t-test. Since, we've found out that our variables are not normally distributed, tried some basic transformations which did not improve the fit. Therefore, we will use a non-parametric alternative, an _independent 2-group Mann-Whitney U Test_, also called a _Wilcoxon rank sum_. The only assumption is that our data is independent and randomly drawn. As we discussed earlier, this is true if our data is all for different randomly drawn companies and not the same one over time. We have no reason to assume otherwise, although the documentation is not clear.
```{r}
wilcox.test(marketing$newspaper,marketing$facebook, conf.int = TRUE)
```
$P-value < 0.05$, therefore we reject the null hypothesis that the mean budgets for the two media are the same. In fact, we find that facebook budgets are higher by approximately between USD 2,000 and 10,000 (remember that our data is in thousands) on a 95% confidence level. 

For a video featuring a non-parametric test for different data in R, check out the one below.

<iframe width="560" height="315" src="https://www.youtube.com/embed/LuWjx0_-VW0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  
## 9. Data visualisation

It seems that we have already used data visualisation extensively for other tasks without ever talking about data visualisation. So, I will assume that you already have some practical experience with graphing but need a general explanation to start creating your own plots without problem. 

The tool we are going to use is `ggplot2` by Hadley Wickham which is based on the _Grammar of Graphics_, developed by Leland Wilkinson. In it, each individual component of a plot is organized into _layers_ which is how ggplots are usually built, with the base `ggplot` function and added (`+`) layers that put geometric objects on the graph, such as `geoms` (e.g. `geom_point` which adds scattered points). Below I give you the general recipe for a ggplot.  
```
ggplot(data = <DATA>) + 
  <GEOM_FUNCTION>(
     mapping = aes(<MAPPINGS>),
     stat = <STAT>, 
     position = <POSITION>) +
  <COORDINATE_FUNCTION> +
  <FACET_FUNCTION> +
  <THEME>
```
No one can explain ggplot better than its creator, so I really really encourage you to read the chapter in the book by, you guessed it, Hadley Wickham. [Jump to chapter 3 here](https://r4ds.had.co.nz/data-visualisation.html). 

Another brilliant reading is "Data visualization: A practical introduction" by Kieran Healy which you might know from lectures. Assuming that you're already familiar with the basics of how to get around in R, I'd suggest starting at [Chapter 3](https://socviz.co/makeplot.html#makeplot) and going as far as possible.

Not instead, but as an addition, before or after reading, you can check out a Data Science Dojo webinar on ggplot. The speaker analyzes a famous dataset about titanic. It features, bar, box and and density plots alongside histograms. It also handles faceting, proper theme and labelling. I embedded the webinar below. It may seem a tad long but the proper R section begins around the 26th minute and 1.25 speed is encouraged. The R file for the session is [here](https://code.datasciencedojo.com/datasciencedojo/tutorials/blob/master/Introduction%20to%20Data%20Visualization%20with%20R%20and%20ggplot2/Data%20Visualization%20with%20ggplot2.R) and the csv can be downloaded [here](https://www.kaggle.com/hesh97/titanicdataset-traincsv).

<iframe width="560" height="315" src="https://www.youtube.com/embed/49fADBfcDD4?start=1615" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

If you want to see examples of some more complicated plots, I recommend checking out [these resources](http://uc-r.github.io/ggplot) from the University of Cincinnati. The first introductory section is somewhat reduntant with the book, but others present some very cool and advanced ideas for visualising many variables at once.

It can be overwhelming looking at all these plots and webinar makers that seem to know the grammar of graphics like the grammar of English or better. Well, fear not, nobody is required to remember any of this by heart, and many don't. [Cheatsheets are not only allowed, but even encouraged.](https://rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf).

![](images/Ggplotcheat.png)

Ggplot2 is incredibly powerful and once you master it, truly beautiful things happen. I leave you with a visualisation of `diamonds`, with 4 variables at once, and yet quite readable. Color is classified alphabetically from D (best) to J (worst). The facets refer to the cut quality.
```{r warning=FALSE, message=FALSE}
ggplot(diamonds,
       aes(x = carat,
           y = price)) +
  geom_point(aes(colour = color),
             alpha = 0.5,
             size = 0.5) +
  scale_color_brewer(palette = "PRGn") +
  geom_smooth(se = FALSE,
              linetype = "dashed",
              colour = "black",
              size = 0.4) +
  facet_wrap(~cut) +
  theme_minimal() +
  labs(y = "price (USD)")

```
  
