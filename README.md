# Wikipedia Size by Language Edition

## Introduction
In this project, I wanted to investigate how different language editions of Wikipedia compare to each other, particularly in how 'large' they are. Here, the idea of 'size' refers to several different measurements of an edition's userbase and amount of content. There are presently 342 active versions of Wikipedia, and 358 in total. I will not be using data for the 16 which have been closed.

## Data Source and Processing
I found my data for this project on the [Wikimedia Meta-Wiki](https://meta.wikimedia.org/wiki/Main_Page), which is... well, their wiki about their wikis. Their "[List of Wikipedias](https://meta.wikimedia.org/wiki/List_of_Wikipedias)" page contains a table called "[All Wikipedias ordered by number of articles](https://meta.wikimedia.org/wiki/List_of_Wikipedias#All_Wikipedias_ordered_by_number_of_articles)," which is, in part, exactly what it sounds like, but also contains other metrics by which you can judge the 'size' of a version of Wikipedia. Including number of articles, they are: 
- count of articles
- count of all pages
  - includes non-articles, like discussion pages
- total edits made
- number of current site admins
- number of site users
- number of active site users
  - users who have made an edit in the last thirty days
- count of locally uploaded files
- editing depth
  - "...one of several possible rough indicators of the encyclopedia's collaborative quality" — see [this page](https://meta.wikimedia.org/wiki/Wikipedia_article_depth) for info. on how it is calculated  

![snapshot](https://github.com/dlaguardia/MATH_144_PDP_DL/blob/main/wikicapture.png)  

Since this table is updated [nine times per day](https://commons.wikimedia.org/wiki/Data:Wikipedia_statistics/data.tab), I used the Internet Archive's Wayback Machine to create a snapshot of the table as it was when I downloaded it, which can be found [here](https://web.archive.org/web/20251203040139/https://meta.wikimedia.org/wiki/List_of_Wikipedias).  

As stated, I then downloaded this table by exporting it as an .[xlsx](https://github.com/dlaguardia/MATH_144_PDP_DL/blob/main/wikipedia_statistics_excel.xlsx) file. At that point, I converted it to a .[csv](https://github.com/dlaguardia/MATH_144_PDP_DL/blob/main/wikipedia_statistics_csv.csv) file, because although RStudio could read both, I prefer the latter. (It's also able to be viewed on GitHub, which is convenient.) To make it workable, I removed:
- the number/rank column (which appears as a question mark in the previous link)
- the "Language (local)" column (as it was redundant)
- the "Wiki" column (unnecessary for analysis)
- the "Depth" column (I would look into this if I had more time, but for now I'm leaving that for the Meta-Wiki)

At this point, all that remained was the "Language" column and the relevant numerical data. I've uploaded the transformed data [here](https://github.com/dlaguardia/MATH_144_PDP_DL/blob/main/wikipedia_statistics_clean.csv).

## Exploratory Analysis
### Scatterplot Matrix (ggpairs)
The visualization below shows a basic scatterplot for every possible pair of variables in our data, as well as the Pearson correlation coefficient measuring the linear correlation between the two variables that make up each of those pairs.   

![scatterplot matrix](https://github.com/dlaguardia/MATH_144_PDP_DL/blob/main/scatterplot_matrix.png)  

Each pair appears to have a very strong positive correlation, which makes sense. A wiki that is larger in one respect will tend to also be larger in all others.  

Additionally, each scatterplot shows one point with both an unusually high x-value and an unusually high y-value. This outlier always corresponds to the English version of Wikipedia. Considering the fact that [nearly half of all web content](https://www.statista.com/statistics/262946/most-common-languages-on-the-internet/?srsltid=AfmBOoozXpPAKVyVbBlqITNw3puRWJ7t3Ke4WGX4FH-w2HDzYae7PPle) uses English as of October 2025, English Wikipedia being much larger than the site's other editions is not surprising.  

Interestingly, each scatterplot in which one variable in the pair was article count also shows one point which appears to only have an unusually high x-value. As may be expected, the article count pairs, being influenced by this second outlier, have weaker correlations.  

### Heatmap (geom_tile)

This is more clearly visualized using a correlation heatmap. Note the immediate visual difference in the tiles representing pairs which include article count — aside from these, the color is almost uniform.    

![correlation heatmap blank](https://github.com/dlaguardia/MATH_144_PDP_DL/blob/main/correlation_tiles_wo_values.png)  

I've also created a version of the above heatmap that includes the actual correlation coefficients which each tile's color is representing, as many are quite similar.  

![correlation heatmap labeled](https://github.com/dlaguardia/MATH_144_PDP_DL/blob/main/correlation_tiles_w_values.png)  

## Analysis: Articles
### Further Exploratory Analysis

To examine what might be going on here more clearly, I created a visualization that displays just the scatterplots for pairs in which one variable is article count. Obviously, this is more legible, and it allows us to see each point more clearly. (In the scatterplot matrix, some detail was sacrificed for the sake of displaying more information at once.)  

![articles plots](https://github.com/dlaguardia/MATH_144_PDP_DL/blob/main/articles_only_scatterplots.png)  

The outlier with an unusually high x-value, visible again here, may suggest that there is one Wikipedia with an article count far greater than expected from the size of its userbase and the amount of non-article content it contains. It turns out that this is true: Wikipedia in [Cebuano](https://en.wikipedia.org/wiki/Cebuano_language), an Austronesian language native to the Phillipines with an estimated 20 million speakers, contains 6115852 articles as of our snapshot. This is the second-highest article count for any edition.  

### Re-Analyzing Correlation

When this outlier — Cebuano Wikipedia — is removed from the dataset, the heatmap noticeably changes.  

![no cebuano wiki heatmap blank](https://github.com/dlaguardia/MATH_144_PDP_DL/blob/main/nocebwiki_correlation_tiles_wo_values.png)  

Once again, this becomes even more evident when labeled with the Pearson correlation coefficients themselves.  

![no cebuano wiki labeled](https://github.com/dlaguardia/MATH_144_PDP_DL/blob/main/nocebwiki_correlation_tiles_w_values.png)  

### Regression and Model Predictions
```
lm(formula = Articles ~ METRIC, data = labeledscatterdata)
predict(MODEL, data.frame(METRIC=c(ACTUAL)))
```
![model](https://github.com/dlaguardia/MATH_144_PDP_DL/blob/main/articles_scatterplots_cebanalysis.png)

|   |All.pages|Edits|Admins|Users|Active.users|Files|
|---|---------|-----|------|-----|------------|-----|
|Predicted|1650391|358997.2|161855.6|156380|144828.8|126026.1|
|Observed|11230188|36794200|7|134536|192|3|

## Conclusion
