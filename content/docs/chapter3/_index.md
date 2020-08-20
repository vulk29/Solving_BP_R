
---
# Title, summary, and page position.
linktitle: Chapter 3
summary: title_goes_here.
weight: 1
icon: book-reader
icon_pack: fas

# Page metadata.
title: Chapter 3
date: "2018-09-09T00:00:00Z"
type: book  # Do not modify.
---

## Motivation

In this section we will explore how to work with datasets which are not perfect and need some 'cleaning' before they can be used for business purposes, like reporting. But, even if it _is_ clean you're gonna need to know how to organize it. 

You're gonna need to install a package of packages for that if you haven't already. As you might have guessed, it's `tidyverse`, your one-stop shop for getting started with manipulating data. Mind you, there is a _raging_ debate on whether to start with tidyverse or base R, and I do encourage you to check it out if you want to dig deeper. Both have their pros and cons, here, you are gonna learn the _tidyverse_ right away for faster results, but bear in mind that [there is another way.](https://github.com/matloff/TidyverseSkeptic).

Okay, now that that's out of the way, let's install the tidyverse.
```
install.packages('tidyverse')
library(tidyverse)
```
Having done that, see the cheatsheet below of the most important organizing functions and their purpose.
```
filter() # select only the rows that match your criteria
arrange() # sort the rows by one or more columns (or more complex)
select() # select only the columns you need
mutate() # add new columns, fx by making operations on the others
group_by() # groups the data frame to individual columns
summarise() # used with group_by to get summaries (mean, sum) for groups
```
Note that all the `dplyr` verbs do not change the original dataset, and if you use them _without_ assigning them to variables like this:
```
filter(iris, Species == "setosa") # filter only the flowers of 'setosa' species
```
We can now look at our `iris` dataset to see whether it has changed.
```
View(iris) # to op4n a tab with the dataset
str(iris$Species) # to see if we dropped any Species from the column
```
It's clearly remained whole. Therefore, whenever you use `dplyr` verbs remember to assign the new data frames to new variables, if you're gonna use them later. 


## dplyr resources

For a comprehensive cheatsheet with more verbs, look to the RStudio's materials on `dplyr`, which is the tidyverse package all these functions belong to. 

![](https://d33wubrfki0l68.cloudfront.net/db69c3d03699d395475d2ac14d64f611054fa9a4/e98f3/wp-content/uploads/2018/08/data-transformation.png)
[I suggest you bookmark it for future use. I did.](https://d33wubrfki0l68.cloudfront.net/db69c3d03699d395475d2ac14d64f611054fa9a4/e98f3/wp-content/uploads/2018/08/data-transformation.png)  

As always, I refer you to "R for Data Science", the material needed for understanding this section is [Chapter 5](https://r4ds.had.co.nz/transform.html) on Data Transformation.

The resource section of course also caters to the perpetual YouTube viewer, with Data School's intro do dplyr which covers all the bases accessibly. It also comes with a [website similar to the one you're on](https://rpubs.com/justmarkham/dplyr-tutorial), that makes it easier to follow along. Save time by watching this video with _1.25_ times the original speed :-)

<iframe width="560" height="315" src="https://www.youtube.com/embed/jWjqLW-u3hc" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Data collection, cleaning and manipulating datasets
It might very well happen that you will need to do some legwork in order to start using your dataset for gathering insights. It is often said that data scientists spend up to 80% of their time cleaning data ([Dasu and Johnson 2003](https://onlinelibrary.wiley.com/doi/book/10.1002/0471448354)), which is important to keep in mind when practicing data science as well as managing a team of data scientists. An additional important note is that tidying refers to arranging datasets such that each variable is a column and each observation (or case) is a row ([Wickham, 2014](https://www.jstatsoft.org/article/view/v059i10)), data cleansing or cleaning has many definitions and can either refer to a general process of "transforming raw data into consistent data that can be analyzed"([de Jonge and van der Loo, 2013](https://cran.r-project.org/doc/contrib/de_Jonge+van_der_Loo-Introduction_to_data_cleaning_with_R.pdf)) or more specifically to removing/fixing duplicates, corrupted, incomplete data ([Müller and Freytag, 2003](http://www.dbis.informatik.hu-berlin.de/fileadmin/research/papers/techreports/2003-hub_ib_164-mueller.pdf)). This section will mostly deal with tidy data and later introduce merging of datasets. 

## Tidying data 

Most commononly encountered problems with data sets include:  

1. Observations are mixed with variables  
  + some of the rows should be columns or the other way around  
2. Multiple columns should be stored as one  
  + separate day, month, year  
3. One variable stores two or more pieces of data and should be split in two.  
  + two sentences separated in two  

And many more that are outlined and discussed in an [article on tidy data that can be found on CRAN](https://cran.r-project.org/web/packages/tidyr/vignettes/tidy-data.html). The article provides guidelines and solutions for frequently encountered problems as well as ready-made code chunks. 

For an even more detailed and theoretically comprehensive outlook on tidy data look to [chapter 12 of the book](https://r4ds.had.co.nz/tidy-data.html) and do the exercises. Should that not be enough, you can turn to [Hadley Wickham's academic article](https://www.jstatsoft.org/article/view/v059i10) in the Journal of Statistical software.

The most important functions are captured in the cheatsheet below, which you can [download here](https://rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf).

![](images/Tidycheat.png)

In terms of video content, I'd recommend a pretty specific video cleaning a sales promotion dataset. It's straightforward but also quite a narrow example, it's only 23 minutes and the teacher is engaging, do give it a try.

<iframe width="560" height="315" src="https://www.youtube.com/embed/wisqb4BFmEY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  
  
It showcases an important feature of this part of a data scientist's job. Namely, that all tidy datasets are the same, but all messy ones are messy in their own way, so there is a lot of extra non-routine work involved in figuring out exactly how to fix any specific case. That said, we're programming here, so once you've solved it and a new batch of data comes through next month, you can just run the same script on it or even have the machine do it for you every Monday. How cool is that? 

Very cool.

## Merging datasets

Sometimes when you're doing analysis you might collect your data from multiple sources and then put it together, for example sales data from multiple branches of the business that is being sent every week by managers to the data analysis team. Good news is you can very effectively use tidyverse for that purpose, bad news is that it often will require some work if the two datasets are not standardized in the same way. Therefore, what you want to do for each of them first is ensuring that they are all tidy and then merging them easily into a master data frame that will inherently also be tidy. In other words, if you add tidy and tidy together you get tidy. On the other hand, if you add messy and messy, you get a whole new kind of messy that can be very difficult to clean.

For a video introduction to merging, check out the webinar from RStudio itself, I recommend watching the whole thing for good fundaments of using dplyr and refreshing on some stuff already covered. The merging part begins at 41:50, see the embed below.

<iframe width="560" height="315" src="https://www.youtube.com/embed/y9KJmUGc8SE?start=2509" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
