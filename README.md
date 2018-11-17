# DSCI-522_nhl-game-predictor
Model to predict win-loss in NHL games.

## Introduction
NHL hockey games are notoriously difficult to predict. There is [common acceptance](https://www.nhlnumbers.com/2013/08/01/machine-learning-and-hockey-is-there-a-theoretical-limit-on-predictions) among hockey analytics enthusiasts that it is not possible to do better than 62% accuracy (i.e. 38% is due to luck). Interestingly, the NHL has recently [announced a partnership](https://www.nhl.com/news/nhl-mgm-resorts-sports-betting-partnership/c-301392322) with MGM Resorts to enable sports betting.

## The Data
The NHL has committed to providing detailed player tracking data starting next season, but in the meantime, we will see how accurate we can be with less granular data.

Plenty of data is available publicly on the [NHL website](www.nhl.com), but we've managed to avoid scraping ourselves and obtained the data directly from [Martin Ellis on Kaggle](https://www.kaggle.com/martinellis/nhl-game-data).

There are several tables of interest here, but we will initially be focussed on the `game_teams_stats.csv` table, which contains the following variables:
- game_id
- team_id
- HoA
- won
- settled_in
- head_coach
- goals
- shots
- hits
- pim
- powerPlayOpportunities
- powerPlayGoals
- faceOffWinPercentage
- giveaways
- takeaways

Notably, this data includes all games starting with the 2012-13 season through the end of the 2017-18 season.

We've created a python script to load the data for our team of interest (Vancouver Canucks, team_id=23) and show the first 10 rows of data:

[Link to the Python script](https://github.com/UBC-MDS/DSCI-522_nhl-game-predictor/blob/master/source/get_data.py)

<center><img src = "imgs/data_snippet.png"></center>

## The Question

**Will the Vancouver Canucks win or lose their next game?**

We want to use all of the data that would be available before the start of a game (i.e. no data from within that game or future games) to predict whether they will win or not.

Question type: **Predictive**

## The Plan

We will use a decision tree to make our prediction. We want to predict the `win` column in the above dataset being either TRUE or FALSE.

We can use `HoA` (Home or Away), `head_coach` and opponent `team_id` as possible factors in our supervised learning, but will need to do some data wrangling to aggregate the other fields from prior games (both for the Canucks and their opponents).

In particular, we think using moving averages (simple and/or recency weighted) will be useful with nearly all of the other fields being numerical. We will have to experiment with how to choose weights and how far back we want them to look.

Another binary data point that we can infer from the data is whether one or both of the teams played a game the day prior. The accepted wisdom on this is that tired teams will have a harder time winning the second game of a "back-to-back" situation like this.

As an extension, if time permits, we will extend the analysis to include additional data outside the identified dataset, such as:
- date / time / timezone of game
- goalie stats
- individual player stats

## Exploratory Analysis

In our exploratory analysis, we plan to look at our data and the distribution of variables in our data. We have some hypotheses in mind that we would test using univariate and bivariate analyses on the relevant variables. For instance, the Home team is more likely to win than the Away team in a game. We will test our hypotheses through the data which would help us later in building our model where we can focus on variables which are the strongest predictors of a win in a game.

A number of approaches can be used here including and not limited to summarizing and looking at averages and counts of different variables like winPercent, goals, shots, etc., visualizing relationships between two variables using correct choices of plots and even looking at the distributions of individual variables through histograms and bar charts.

## Final Results

To present the final results, we plan to use visualizations for our model such as visualizing the decision tree obtained as a result of the model building process. We will be using cross-validation to get a validation accuracy in addition to the testing accuracy which will help us gauge how robust our model is to overfitting. As an output, we will show our training, validation and test accuracies and comment on how generalizable our model is based on these numbers. Another focus in our results would be on the importance of features in our model and the comparison of the strongest predictors obtained to our original hypotheses.
