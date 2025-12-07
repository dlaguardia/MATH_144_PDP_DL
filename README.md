# Wikipedia Size by Language Edition

## Introduction
## Data Source and Processing
I found this data on the [Wikimedia Meta-Wiki](https://meta.wikimedia.org/wiki/Main_Page), which is... well, their wiki about their wikis. Their "[List of Wikipedias](https://meta.wikimedia.org/wiki/List_of_Wikipedias)" page contains a table called "[All Wikipedias ordered by number of articles](https://meta.wikimedia.org/wiki/List_of_Wikipedias#All_Wikipedias_ordered_by_number_of_articles)," which is, in part, exactly what it sounds like, but also contains several other metrics by which you can judge the 'size' of a version of Wikipedia: 
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
As this table is updated [nine times per day](https://commons.wikimedia.org/wiki/Data:Wikipedia_statistics/data.tab), I used the Internet Archive's Wayback Machine to create a snapshot of the table as it was when I downloaded it, which can be found [here](https://web.archive.org/web/20251203040139/https://meta.wikimedia.org/wiki/List_of_Wikipedias).

## Exploratory Analysis
### Scatterplot Matrix (ggpairs)
![scatterplot matrix](https://github.com/dlaguardia/MATH_144_PDP_DL/blob/main/scatterplot_matrix.png)
### Heatmap (geom_tile)
![correlation heatmap blank](https://github.com/dlaguardia/MATH_144_PDP_DL/blob/main/correlation_tiles_wo_values.png)
![correlation heatmap labeled](https://github.com/dlaguardia/MATH_144_PDP_DL/blob/main/correlation_tiles_w_values.png)
## Analysis: Articles
### Further Exploratory Analysis
![articles plots](https://github.com/dlaguardia/MATH_144_PDP_DL/blob/main/articles_scatterplots.png)
### Re-Analyzing Correlation
![no cebuano wiki heatmap blank](https://github.com/dlaguardia/MATH_144_PDP_DL/blob/main/nocebwiki_correlation_tiles_wo_values.png)
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
