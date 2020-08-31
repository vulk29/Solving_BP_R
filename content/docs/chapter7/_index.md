---
# Title, summary, and page position.
linktitle: Chapter 7
summary: Clustering and regressions (recap)
weight: 7
icon: book-reader
icon_pack: fas

# Page metadata.
title: Chapter 7
date: "2018-09-09T00:00:00Z"
type: book  # Do not modify.
---
## Cluster analysis

The next topic to cover is the first one that deals more with math than it does with programming. Cluster analysis and regression analysis are the non so fancy terms for 'unsupervised' and 'supervised' learning. Well, that is not entirely true, but it is also not entirely untrue. That is why, you need to make sure you have the basics right. It's also important to be able to run these things in R.

Cluster analysis is finding subgroups of observations within a data set. Usually if we plot a few observations based on two variables, we can do this easily by "eyeballing" them, but computers can't. They don't know how many clusters to build or how to put them together. If we manage to teach them that, we can exceed the ability of our own eyes and include many more variables and have much better clusters. Cluster analysis is what computer scientists would call an "unsupervised learning method", because there is no response variable ("y") and the algorithm instead of predicting something, tries to find similarities in the existing data and group it. Incidentally, cluster analysis is also _probably_ your very first **machine learning** model, so that's one buzzword off the bucket list.

On a serious note, as you might already know, there are two most popular approaches to algorithmically solving the clustering problem, *k-means* and *hierarchical clustering*. In this section I will focus on what is probably the most popular one, *k-means*. ([Seif, 2018](https://towardsdatascience.com/the-5-clustering-algorithms-data-scientists-need-to-know-a36d136ef68))  
  
Now, as I stated before, it is more of a mathematical challenge than it is a programmatic one. Once one understood both of the methods and can apply one, R offers packages (e.g. `cluster`) with built in functions that combined form a ready-made recipe for performing cluster analysis. 
  
If you feel rusty on the theoretical side and would like to code along a more math-heavy example, I recommend [this article from the University of Cincinnati](https://uc-r.github.io/kmeans_clustering) (there are other great resources on the same page which are worth checking out).  
  
For fast results and an easy walk-through check out the Youtube video below. In the beginning it deals with important measures to take to prepare one's dataset for clustering and then dives straight into the hierarchical method to later emerge with a *k-means* around [14:45](https://youtu.be/5eDqRysaico?t=885).

<iframe width="560" height="315" src="https://www.youtube.com/embed/5eDqRysaico?start=885" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### K-means clustering example

You might recall the `iris` dataset we've talked about above. Let's try performing clustering on it and do a walk-through. First, if your machine doesn't have the packages, we better install them. PS we're also gonna use tidyverse but I assume by now it's all installed.

```
install.packages(c("cluster", "factoextra"))
```
We're also gonna load them once we're at it.
```{r warning=FALSE, message=FALSE}
library(tidyverse)
library(cluster)
library(factoextra)
```
Now we can get onto the first step which is loading our data which in this case is built-in. We'll assign it to a variable to store it and perform operations on it.
```{r}
df <- iris
```
Now, as you probably remember, `iris` has 5 variables, the first four are continuous and store information about widths and lengths of sepals and petals, the fifth is a categorical variable that stores species. The first step before doing clustering is often to plot any two continuous variables that we might suspect have an illustrative relationship and try to 'eyeball' the clusters. In this special case it would not be crazy to assume that clusters could be species of irises, therefore I will add a colour to see whether that's a reasonable assumption. I also add a `theme_bw`, because I can't stand the grey default of ggplots. Highly recommend.
```{r}
ggplot(data=iris, mapping = aes(y = Petal.Length, x = Petal.Width)) + 
  geom_point(mapping = aes(color = Species))+
  theme_bw()
```

Here we see that the Species mostly follow what we'd expect by eyeballing in terms of the Petals. This is of course a subjective exercise but if I were gonna cluster the flowers with my eyes based only on those two variables and without knowing about the Species, it'd look something like this. 

```{r}
ggplot(data=iris, mapping = aes(y = Petal.Length, x = Petal.Width)) + 
  geom_point(mapping = aes(color = Species)) +
  geom_point(data=iris[iris$Petal.Width<0.75,],
             pch=21, fill=NA, size=3, colour="red", stroke=1) +
  geom_point(data=iris[iris$Petal.Width>0.75&iris$Petal.Width<1.75,],
             pch=21, fill=NA, size=3, colour="green", stroke=1) +
  geom_point(data=iris[iris$Petal.Width>1.75,],
             pch=21, fill=NA, size=3, colour="blue", stroke=1) + 
  theme_bw()
```
  
As you can see it's mostly the same as the Species, apart from some Virginica and Versicolor tension there where some blue points get encircled by the green cluster. Eyeballing is a good exercise to have an idea about the dataset and how many clusters do we expect there to be. Now, we want to see how the computer tackles this task based on 4 continuous variables, (we can only effectively eyeball 2 at once). How many clusters will our k-means have and how will they look like? Let's find out. 

First, we need to prepare the data set, df we set above for analysis. For that I will remove any categorical variables, Species, which is the fifth column of our dataframe. Then, we need to _standardize_ the other metrics so that they have a mean of 0 and a standard deviation of around 1. The `scale` function takes care of that by centering and scaling.
```{r}
df <- scale(df[,-5])
```
Now we have to compute the distance between our observations, for which we'll use the `factoextra` package. `get_dist` automatically computes said distance for us, with the default being the standard Euclidian version $\begin{equation} d_{euc}(x,y) = \sqrt{\sum_{i=1}^n(x_i - y_i)^2}\end{equation}$. That's the one we will use, but be advised that [other distance metrics exist](https://www.datanovia.com/en/lessons/clustering-distance-measures/). After that we will visualise said distances on a heatmap.
```{r}
distance <- get_dist(df)
fviz_dist(distance,show_labels = FALSE)
```

This visualisation yields that there are large differences between flowers in this dataset (purple) and there are also a fair amount of these that appear to be a part of the same cluster (red). I don't show the labels of the observations, because individual flowers have no meaningful names, but it is a perfectly sound thing to do with fewer observations that have meaningful labels, e.g. EU countries.

Now that we know clusters exist, we have to determine the number of clusters (k) to use the k-means algorithm for clustering. We'll use what's called, the elbow method to visualise the within clusters sum of squares, WSS, for different numbers of clusters, k. 

```{r}
set.seed(123)
wss <- 1 #placeholder
for (i in 1:10) wss[i] <- sum(kmeans(df, centers=i)$withinss)
ggplot(mapping = aes(1:10, wss))+
  geom_point()+
  geom_line()+
  scale_x_continuous(name = "Number of clusters", breaks = seq(0,11,1))+
  scale_y_continuous(name = "Within cluster sum of squares")+
  theme_bw()
print(wss,digits = 0)
```
This is somewhat of a complicated set of commands, so let's go through them one by one. First, we set seed so that the kmeans algorithm that we will run will come at the same result every time we run this code, because there is some randomness to the wss value for clusters. Then we define _wss_ as the sum of squares variation for values of k from 1 to 10 and then we plot it for values of k. Notice that when the `kmeans` function's argument `centers` effectively means k, number of clusters.     
Lastly, we look for the "bend" or "elbow" in the data - the point where adding more clusters does not monumentally decrease _wss_. Here it seems to be 3 or 4. Having the benefit of knowing about there being three Species, I will choose 3 going forward, but it's close, as you can see both based on the plot and the printout of _wss_ values. 

You can also use the function built-in the `factoextra` package, that does the same thing but in a less customizable fashion and without letting you know what calculation is going on in the background. Its arguments are the normalized dataset `df`, algorithm  `kmeans` and method `"wss"`.
```{r}
fviz_nbclust(df, kmeans, method = "wss")
```

Now that we've determined using the elbow method that the appropriate number of clusters is 3, we can see how does the algorithm divide them based on all four variables. We visualise the results using the function `fviz_cluster` which does that neatly for us. 
```{r}
iris_cluster_3 <- kmeans(df,centers = 3, nstart = 25)
fviz_cluster(iris_cluster_3,data=df,ggtheme = theme_bw())
```
  
Here we have the clusters visualised over the principal dimensions that contributed to variability the most. Although this looks good we might be interested in how that does compare to our initial plotting of the flowers based on Petal lengths and widths by Species. It is also interesting to see whether the Species are the clusters.

```{r}
iris_clustered <- iris %>% mutate(cluster = iris_cluster_3$cluster)
ggplot(data = iris_clustered, aes(Petal.Width,Petal.Length)) + 
  geom_point(mapping = aes(color = Species)) +
  geom_point(data=iris_clustered[iris_clustered$cluster==1,],
             pch=21, fill=NA, size=3, colour="red", stroke=1) +
  geom_point(data=iris_clustered[iris_clustered$cluster==2,],
             pch=21, fill=NA, size=3, colour="green", stroke=1) +
  geom_point(data=iris_clustered[iris_clustered$cluster==3,],
             pch=21, fill=NA, size=3, colour="blue", stroke=1) + 
  theme_bw()

```
  
Based on this we can see that the algorithm has mostly correctly clustered the flowers into Species, and that setosa is much different from the other two which have some overlap. Bear in mind that the computer has not only considered the petals which I visualised here but also the other two variables in making the clustering decision. 

As a last note, we might want to investigate exactly how correctly the flowers were clustered.
```{r}
table(iris_clustered$Species,iris_clustered$cluster)
```
Now that we have the table we can calculate the number of correctly clustered flowers and the success ratio. 
$$ rate_{success} = \frac{successes}{nrow(iris)} = \frac{50+39+36}{150} \approx 83.3\% $$
It looks like our clustering was about 83% correct. Of course most of the time we're not gonna have such a neat measure of success as with the flowers example but it well illustrates that clustering is effective and useful. What if the species classification was lost from the data?

## Regression analysis

You've probably heard of modeling before, it seems that nowadays everyone is answering all questions they have with _fitting a model_, _modeling a relationship_ or simply trying to _predict the sales_. Well the days of being left out of this conversation are over and we're now going to start modeling ourselves. We're gonna start with a detailed walk-through of a simple linear model and then go onto a multiple regression.

### Simple linear regression

The first type of regression we'll tackle is, well, simple. It is going to try and model the relationship between two variables, one _dependent_ and one _independent_, which also gets called _explanatory_ or a _predictor_. So, first we have to determine whether there is any ground to assume that a relationship between two variables exists, that they are _correlated_. Note that we do not require one to be _caused_ by the other. Although that might be the case, the general rule is:
  
> Correlation does not imply causation!

So let's say we are interested in whether there is a relationship between sales of a company and the advertising budget, specifically how and if the budget can affect the sales. Hence sales will be our _dependent_ variable and advertising budget will be our _independent_ variable. See the sample regression line:
$$\begin{equation}
\hat{Sales}_i = \hat{\beta}_0 + \hat{\beta}_1 {Budget}_i + \hat{\epsilon}_i
\end{equation}$$

I got the dataset `marketing` from a package called `datarium`. It has a default of 4 variables, 3 that represent budgets for different advertising media and one for sales. Because we're doing a simple regression, I will create a new column, `marketing$Budget`, that stores the total advertising budget. Then we can inspect whether there is grounds to think that a relationship exists. Then I perform a regression using the `lm` function and store it in a variable `marketing_model`. Notice the syntax of `lm` is `lm(dependent ~ independent, data)`.
```{r warning = FALSE, message = FALSE}
library(datarium)
marketing_df <- marketing %>% mutate(budget = rowSums(marketing[,1:3]))
ggplot(marketing_df,aes(budget,sales))+
  geom_point() + stat_smooth(method = lm, se = 0) + theme_bw()
marketing_model <- lm(sales ~ budget, marketing_df)
```
  
Later a very important step before we can start using the model we built in the last line is checking the *assumptions* of our chosen technique - simple linear regression. R makes it very easy with the built in `plot` function that gives us the four plots we need. [Read more about assumptions here.](http://people.duke.edu/~rnau/testing.htm)
```{r}
par(mfrow = c(2, 2)) #show plots next to each other 2x2
plot(marketing_model)
```
  
Based on the first plot, we can declare that there is indeed a linear relationship between the two variables because the residuals are evenly distributed around subsequent fitted values. You can easily examine that by looking at the red line, if it sticks to 0, there is no problem. Due to fewer observations towards the higher Sales values the relationship shifts slightly.   
The second plot shows us quite clearly that the vast majority of residuals are normally distributed.  
The third plot shows no clear pattern of variance which is desirable.However, it is not an ideal situation in which the red line would be entirely horizontal, indicating equal spread of variance along the values of $y$. 
On the fourth plot, influtential points are shown, although there is no reason to assume that any outliers need to be removed, especially since linear regression is a robust method. [You can read more here.](https://www.jstor.org/stable/2286747?seq=1#metadata_info_tab_contents)
The last assumption of the regression is independence of residuals, we can assume that the entries in this dataset are independent of one another if it is different companies. However, the documentation of the dataset does not shed light on this.

Now, that we got assumptions out of the way, and they all seem to be satisfied we can examine our model, it's significance and how much of the variance it explains.
```{r}
summary(marketing_model)
```
Additionally, let's look at the mathematical representation of the model:
$$\begin{equation}
\hat{Sales}_i = 5.0916 + 0.0487 {Budget}_i + \hat{\epsilon}_i
\end{equation}$$
Looking at the p-values of the coefficients (all under 0.001) and the R-squared, we can confidently fail to reject the hypothesis that advertising budget has no effect on Sales. Our model's R-squared yields that the budget explains around 75% of the variation, which is remarkably high. We can read the model's _budget_ coefficient as 'For each 1000 USD spent on advertising, approximately 48.7 USD returns in form of sales'. That's not terribly efficient, but maybe the companies are in the process of building a customer base and therefore spend a lot on creating awareness. Remember that there is also the intercept which on its own should not be interpreted, but will be adding 5000 USD to a predicted _Sales_ value. This model is of course not perfect and there is other possible pitfalls such as the [ommitted variable bias](http://hedibert.org/wp-content/uploads/2016/09/Bias-omittedvariable.pdf).

Additionally, the summary of the model output in R is explained in StatQuest's short video.

<iframe width="560" height="315" src="https://www.youtube.com/embed/u1cc1r_Y7M0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Multiple regression

Let's say your boss looks at the report of your results from the previous section and e-mails you that he'd like to know which of the media mix is most efficient in impacting the sales based on the data. There, you're looking at a multiple regression problem, with one dependent variable, same as before, _Sales_, and three independent variables, budgets of the three channels your company advertises through, _youtube_, _facebook_ and _newspaper_. First, you write out the model:
$$\begin{equation}
\hat{Sales}_i = \hat{\beta}_0 + \hat{\beta}_1 {youtube}_i + \hat{\beta}_2 {facebook}_i + \hat{\beta}_3 {newspaper}_i + \hat{\epsilon}_i
\end{equation}$$
Then you dive straight into R.
```{r}
marketing_model_multiple <- lm(sales ~ youtube + facebook + newspaper, marketing_df)
```
Then the discussion of assumptions should follow, but seeing as we've discussed them in detail above, I will only focus on the one assumption that occurs in a multple regression and does not in a simple linear. Said assumption is independence of variables, or lack of multicollinearity. There are several ways of invetigating that, I will use a simple correlation plot, but you may also want to [watch the VIF method](https://www.youtube.com/watch?v=I4z3yjoEADY). 
```{r warning=FALSE, message=FALSE}
library(corrplot)
corrplot(cor(marketing_df[,c(1:3)]), method = "circle")
```
  
Here we see that there is some multicollinearity, especially between facebook and newspaper, which is over 0.35. Multicollinearity could result in bloated standard errors and in turn, larger p-values for the two variables. Although it is worth noting that it does not affect the overall model fit, so R-squared or F does not get distorted. Additionally, as a rule of thumb, any correlation between the variables $r\leqslant0.9$, [should not be worrying](https://stats.stackexchange.com/questions/100175/when-can-we-speak-of-collinearity), so we can safely proceed. Let's draw up the summary of the model then:
```{r}
summary(marketing_model_multiple)
```
Newspaper looks to be highly insignificant, therefore we can safely drop it and run a restricted version of the model.
```{r}
summary(lm(sales ~ youtube + facebook, marketing_df))
```
$$\begin{equation}
\hat{Sales}_i = 3.5053 + 0.0458 {youtube}_i + 0.1880 {facebook}_i + \hat{\epsilon}_i
\end{equation}$$

We can quickly determine that this model is better at capturing the variation in _Sales_, than the previous simple linear one, because of the higher R-squared, lower RSE and higher F-statistic. The coefficients next to the independent variables can be interpreted as such, 'For each 1000USD spent on YouTube advertising, sales rise by 45.8USD. Analogically for Facebook adverts, sales rise by 188USD'. Now you can present the results to your boss, which _indicate_ that Facebook is the most efficient medium, while it couldn't be rejected that newspaper budget has no influence over sales. You might also want to add that possibly there are other variables that you don't have access to in play, so he should ask other people in the company for input before pulling all the money from the local newspaper to invest it in Facebook ads. 

If you'd like watch a video on a different multiple regression example in R, I recommend the one below.

<iframe width="560" height="315" src="https://www.youtube.com/embed/S-zKhFr91Tg" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Additionally, if you're interested in how to use R to model, with a much broader set of tools, as always, [I refer you to Hadley's book](https://r4ds.had.co.nz/model-basics.html).

