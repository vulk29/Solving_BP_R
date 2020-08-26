---
# Title, summary, and page position.
linktitle: Tutorial 9
summary: Text Mining/text analysis in R.
weight: 9
icon: book-reader
icon_pack: fas

# Page metadata.
title: Chapter 9
date: "2018-09-09T00:00:00Z"
type: book  # Do not modify.
---


## Text Mining/text analysis in R.

Load a sample from twitter into your R. You have two options:
* First option: Download a new dataset from twitter:

  For gathering a dataset from twitter you need a regular twitter account and an "academic researcher account" (https://developer.twitter.com/en/solutions/academic-research). Please note that the setup of the latter requires some waiting. Twitter will manually approve the purpose of your research before they open the account. This takes roughly a day.
  
  If you decide so, please search for some characteristics of tweets, for example, hashtags in certain time period and download this data to your machine.

* Second option: Use the dataset we have prepared: https://www.dropbox.com/s/dlxmnluoogogrv6/twitter001.csv?dl=0 
  This is a csv file, tabs are used as separator.

Please answer the following questions:

(1.1) What attributes/features does the dataframe have? How many data records are there?

(1.2) What do you expect is the correlation between the number of followers and the number of followings, specifically do you expect a positive or negative correlation? Why? Test the correlation with the you decided to use. Could you support or reject your expectation?

(1.3) Calculate a linear regression estimating the number of followers on the number of followings. Do get the same direction as in the correlation analysis? What can the regression model tell us about the data beyond what we know from the correlation analysis?

(1.4) Please check the assumptions you made until here. Check the number of all users in the dataset. Is it the same as the number of unique users? (Hint: it should since you use searched for tweets, not users.) If not, did you consider this in the analyses (points 1.2 and 1.3)? Why and how do you have to account for multiple records of users? Adjust the data and recalculate the correlation and regression. (Hint the package plyr can help here.) What has changed (compared to the original results)? Plot the relationship between followings and followers.

(1.5) Calculate the length of the twitter name (nickname) by counting the number of characters for each user. Does this variable predict the number of followers?

Sentiment analysis:
(2.1) Install and load the R library "sentimentr" and calculate the sentiments of all tweets with the function "sentiment()". How long did the calcualtion take? (For advanced students: try to write a function that prints the time the function needed.) What is the outcome of the function? Decribe briefly. Try to merge the outcome with orginal data (tipp: you can use the function cbind()). 

(2.2) Calculate a regression estimating 



