
Data visualisation

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
  
