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

  
