## Text Mining/text analysis in R.

Load a sample from twitter into your R. You have two options:
* First option: Download a new dataset from twitter:

  For gathering a dataset from twitter you need a regular twitter account and an "academic researcher account" (https://developer.twitter.com/en/solutions/academic-research). Please note that the setup of the latter requires some waiting. Twitter will manually approve the purpose of your research before they open the account. This takes roughly a day.
  
  If you decide so, please search for some characteristics of tweets, for example, hashtags in a given time period and download this data to your machine.

* Second option: Use the dataset we have prepared: https://www.dropbox.com/s/dlxmnluoogogrv6/twitter001.csv?dl=0 
  This is a csv file, tabs are used as the separator.

Please answer the following questions/do the following tasks:

(1.1) What attributes/features does the dataframe have? How many data records are there?

(1.2) What do you expect is the correlation between the number of followers and the number of followings, specifically do you expect a positive or negative correlation? Why? Test the correlation with the you decided to use. Could you support or reject your expectation?

(1.3) Calculate a linear regression estimating the number of followers on the number of followings. Do get the same direction as in the correlation analysis? What can the regression model tell us about the data beyond what we know from the correlation analysis?

(1.4) Please check the assumptions you made until here. Check the number of all users in the dataset. Is it the same as the number of unique users? (Hint: it should since you use searched for tweets, not users.) If not, did you consider this in the analyses (points 1.2 and 1.3)? Why and how do you have to account for multiple records of users? Adjust the data and recalculate the correlation and regression. (Hint the package plyr can help here.) What has changed (compared to the original results)? Plot the relationship between followings and followers and interpret the plot.

(1.5) Calculate the length of the twitter name (nickname) by counting the number of characters for each user. Does this variable predict the number of followers?

Sentiment analysis:
(2.1) Install and load the R library "sentimentr" and calculate the sentiments of all tweets with the function "sentiment()". How long did the calculation take? (For advanced students: try to write a function that prints the time the function needed.) What is the outcome of the function? Describe briefly. 

(2.2) Try to merge the outcome of "sentiment()" with original data (tipp: you can use the function cbind(), the result of "sentiment" needs to be aggregated based on their "element_id" before, e.g. using ddply()). Note 2.1 can be difficult and it is great if you came so far!

(2.3) Calculate a regression estimating the the number of followers on the sentiment of the tweets. Write down your expectations before. Which direction (positive or negative or neutral) do you expect? Provide reasons for your expectations and interpret your findings. 

Word cloud:
(3.1) Install and load packages tm, SnowballC, wordcloud, RColorBrewer

(3.2) Read the tweets (i.e., the text data) into a/as Corpus. Therefore, use the functions "VectorSource()" and "Corpus()": Corpus(VectorSource(data$Tweet.content)). Use the "inspect()" function to view the corpus. What do you see?

(3.3) Manipulate the corpus by setting all characters to lower case, removing numbers, removing stopwords, removing punctuation, removing white spaces (blanks), and word stemming. The following code does all these steps (assuming your corpus is named "data.corpus"):
```
data.corpus.01 <- tm_map(data.corpus, content_transformer(tolower))
data.corpus.01 <- tm_map(data.corpus.01, removeNumbers)
data.corpus.01 <- tm_map(data.corpus.01, removeWords, stopwords("english"))
data.corpus.01 <- tm_map(data.corpus.01, removePunctuation)
data.corpus.01 <- tm_map(data.corpus.01, stripWhitespace)
data.corpus.01 <- tm_map(data.corpus.01, stemDocument)
```
Note: there might be warnings.

Then transform the corpus into a TermDocumentMatrix:
```
data.dtm <- TermDocumentMatrix(data.corpus.01)
```
(3.4) Tranforms the resulting TermDocumentMatrix into a matrix (hint: use the function "as.matrix()"). (Note if your TermDocumentMatrix is large, you might get problems with the casting into the matrix. Then, go back to 3.1 and take a subsample by limiting the data, e.g., to a certain hour at a given day.)

(3.5) The matrix should be ordered alphabetically (terms). Create a vector out of the matrix that contains the frequency of the terms and uses the terms as names. It should be ordered by the frequency of the terms. (Tip: This will do: ```sort(rowSums(data.m),decreasing=TRUE)```.) Transform the result in a dataframe with two columns, one of the terms (call it "term") and one for the frequency (call it "frequency").

(3.5) Use wordcloud() to create (draw) the word cloud. What happens if you draw the word cloud a second time (excuting the same command twice)? Why did the arrangement change? How could we fix the word cloud and make it replicable?

