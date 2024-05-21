---
layout: page
title: Using Machine Learning to Predict NBA Score Spreads
description: Carolina Data Challenge
img: assets/img/nba_background.jpg
importance: 1
category: my work
related_publications: false
toc:
  sidebar: left
---

## Introduction
As one of the most popular sports in the world, basketball continues to sweep the attention of casual watchers to die-hard fans. As with most sports, one of the biggest topics in basketball is predicting whether one team will prevail over another. The ability to predict not just the outcome of a game but also other statistical variables like turnovers or free throw percentage also holds a considerable amount of value for analysts, coaches, and even players. And as online sports betting continues to skyrocket in popularity, the incentive of sports analysts to find the best predictive model has never been higher. In this paper, I will take you through the steps I took to create predictive models in R Studio for the point spread between home and away teams. The models will then be used to predict the outcomes of the 57 remaining NBA 2024 Regular Season games, starting on April 9, 2024.

## Data Information

Before the model creation phase, I decided to create different unique datasets to organize the process and provide a clear structure of how it will be used in the models. In this part of the paper, I will strictly explain how I cleaned the data and how the datasets will be used in the methodology section.

### NBA Regular Seasons Game Dataset 

To prepare for predictive modeling of NBA game outcomes, data was retrieved using the [abresler/nbastatR](https://rdocumentation.org/packages/nbastatR/versions/0.1.110202031) package, facilitating access to the NBA Stats API. This API offers extensive, real-time datasets encompassing player and team statistics, game results, and other pertinent metrics.

I obtained the list of every game that happened in the NBA Regular Season from 2012 to 2024 up to April 7, 2024. The uncleaned dataset of the Regular Season Games Dataset has 30,744 rows and 47 variables. Each row of this dataset describes the statistics of one NBA team, meaning for each unique NBA game, there are two rows, one for the home team and one for the away team.

```r
NBA_SEASON_GAMES=game_logs(  
 seasons = 2012:2024, 
 league = "NBA", 
 result_types = "team", 
 season_types = "Regular Season", 
 nest_data = F, 
 assign_to_environment = TRUE, 
 return_message = TRUE 
) 
```
First, I removed some columns in the Games Dataset that were unnecessary or redundant for the analysis.

```r
NBA_SEASON_GAMES_CLEANED_1 = NBA_SEASON_GAMES %>% dplyr::select(-c(countDaysNextGameTeam, nameTeam, idTeam, slugSeason, slugLeague, typeSeason, numberGameTeamSeason, slugMatchup, slugTeamWinner, slugTeamLoser, outcomeGame, isWin, hasVideo, minutesTeam, urlTeamSeasonLogo, isB2B, isB2BFirst, isB2BSecond))
```
The dataset rows were then split to create two datasets based on whether the locationGame was home or away. Then, an inner join was performed based on idGame and dateGame to combine the home and away statistics, ensuring each row represents a unique game. Subsequently, I added “.x” and “.y” to the end of each column name to distinguish between home data and away data, respectively. The dataset now included only the unique game identifier, the home/away location, the team names, and the box score statistics related to the game.

In addition, I also calculated the Spread and Total for each game. For Spread, this was equivalent to the variable plusminusTeam.x in the original dataset. For Total, I computed the total number of points scored in an NBA game by adding ptsTeam.x and ptsTeam.y.

### NBA Player by Game Dataset 
The nbastatR package was utilized again to generate a player-by-game dataset. This dataset included NBA basic player game logs for Regular Seasons 2012 to 2024. Each row includes information on a singular player and their performance in a given game. The dataset features 325,063 observations and 58 columns.

### NBA Division Rank Dataset
NBA division rank statistics for Regular Seasons 2019 to 2024 were obtained using the standings function in the nbastatR package. The uncleaned dataset included 180 rows and 224 columns. I included only the yearSeason, slugTeam, and rankDivision columns. I also noticed that there was inconsistency in the naming convention in the column slugTeam. Specifically, the Denver Nuggets, typically represented by “DEN”, was named “DN” in this dataset. In addition, the Los Angeles Clippers or “LAC” had incorrectly been attributed to a NA name. Thus, I made the necessary changes to align team names with the proper three-character naming convention of the NBA.

### New Variables
One of the reasons I added new variables is to improve the efficiency and accuracy of the models. These new variables will also provide more leverage during the model-creating process, as they can simplify complex relationships not detected easily in the NBA boxscore.

#### Effective Field Goal (EFG)
Effective field goal percentage seeks to provide more context on the shooting ability of a specific team. This statistic is widely used in NBA statistics. Before I provide the formula for EFG, it is important to know how the regular field goal percentage variable is calculated.

$$
\text{Field Goal Percentage} = \frac{\text{Field Goals Made}}{\text{Field Goal Attempts}}
$$

While the FG% statistic is simple and robust, it can be slightly improved. The problem with FG% is that it does not account for the different types of shots a team makes, specifically the 2-pointer vs. 3-pointer. A team that makes only 20 3-pointers in a game is more valuable than a team that makes only 20 2-pointers. This is because 3-pointers are worth more in points. The Effective Field Goal Percentage hopes to fix this problem. By weighing 3-pointers more than 2-pointers, it is more easily perceived whether a team had more “valuable” shots. EFG is as calculated:

$$
\text{Effective Field Goal Percentage} = \frac{0.5 \times \text{Three Point Field Goals} + \text{Field Goals Made}}{\text{Field Goal Attempts}}
$$

#### ESPN Player Rating
One of the data pieces the group had trouble with was incorporating some sort of player statistic into the dataset. I only had the team statistics to go off of, and I believed having information on the players could give an extra edge to the model. For this variable, I used the nbastatR package and an outside source from ESPN to calculate the new variable. 

First, using the dataset found in Section 1.2, I filtered to a specific year I wanted to create the variable for. Next, I calculated ESPN’s Player Rating for each player and added it as a new variable called espnRating  in the dataset.

$$
\text{ESPN Player Rating} = \text{Points} + \text{Rebounds} + 1.4 \times \text{Assists} + \text{Steals} +
$$
$$
1.4 \times \text{Blocks} + \text{Field Goals Made} + 0.25 \times \text{Free Throws Made} -
$$
$$
0.7 \times \text{Turnovers} - 0.8 (\text{Field Goals Attempted} - \text{Field Goals Made}) - 
$$
$$
0.8 (\text{Free Throws Attempted} - \text{Free Throws Made})
$$

This lengthy equation does a good job of weighing certain variables more than others and even accounts for mistakes a player makes, such as turnovers.

What I wanted to do with this variable is to calculate the espnRating of the top five players on every NBA team, take its average, and then insert it into dataframe 1.1. I chose the top five, since these players are most likely the starters of each team, meaning they would play the most minutes. Teams with better players would most likely have a higher espnRating.

#### Overall Team Rank
Knowing whether a team won or not in the 1.1 dataframe allowed us to create a metric to define team rank. To create a team ranking system, I counted the number of wins each team had in a specific season, then I ranked them from highest to lowest, where rank 1 teams were the best in the NBA and rank 30 were the worst in the NBA.

I called this variable Rank, and I implemented it in the 1.1 dataframe.

#### Offensive Ability
Offensive ability was a variable I pursued to scale how each team was able to perform offensively in basketball. Knowing how a team performs offensively vs defensively is important since each aspect is so unique to one another, and not only that, they play equally important roles in basketball. A team that has the best offense, but the worst defense most likely cannot compete with a team with above-average offense and defense. Therefore, I believe creating a variable for this factor would allow the models to be more accurate. The way I calculated offensive ability is shown below.

$$
\text{Offensive Ability} = \text{Offensive Rebounds} + \text{Assists} + 100 \times \text{Field Goal Percentage} + \frac{\text{Points Earned}}{10} - \text{Turnovers}
$$

FG% is multiplied by a factor of 100 in order because regularly its value is between 0 and 1. If I want to have FG% impact the offensive ability score, I need to weigh it considerably higher. The same goes with Points Earned, where I divide by 10 so that it does not explain the majority of the new variable.  I called this variable offAbility, and I calculated it in dataframe 1.1.

#### Defensive Ability
Like offensive ability, defensive ability is the ability of a team to hold an opponent from scoring. I calculate the defensive ability of a team below.

$$
\text{Defensive Ability} = \text{Defensive Rebounds} + \text{Steals} + 2 \times \text{Blocks} + \text{Turnovers}
$$

Blocks do not happen often in games, only around 3-5, so I weighed it twice as much to make it gain more significance. I call this variable defAbility, and I incorporate it into the dataframe in 1.1.

## Methodology for Spread
Spread in basketball denotes the difference between the home and away team’s score. Therefore a home win will display a positive spread, while an away win will display a negative spread. Before I  began finding the best model for basketball spread, OREB, and total, I realized that I had to change the perspective on predicting data. For many models in the past, the goal was to create a method to predict the response variable, given a set of predictors for events that already happened. The difficulty of creating a model to predict the future is that I have no data regarding the predictor variable. 

For the methodology, I will introduce each step of how I was able to overcome the obstacle of not knowing anything about the predictor variable. 

I provided a general summary and brainstorm of how I wanted to calculate the spread predictions.

From the NBA Regular Seasons Game Dataset from Section 1.1, calculate the means regarding each of the NBA teams in terms of the predictor variable into one separate dataset as the summary dataframe.
In a separate dataset, transform the NBA Regular Seasons Game Dataset from Section 1.1 into a table that contains one row for each game with all the box score statistics (blocks, turnovers, field goals attempted, etc.) for each home and away team. In this step, I also create the variable called, Spread, which is the difference between the points scored by the home and away teams.
From the transformed dataset in step 2, we run a stepAIC function to find the best variables that explain the Spread variable. 
Setting  up the training and testing set:
The training set will be the dataset from Step 1 that has been transformed and selected for the best variables from Step 3.
The testing set will be the matchups of the future NBA games starting April 9, 2024. However, in place of the box score statistics, I will use the dataset in Step 1 containing the summary statistics of each team. For instance, if on April 10th, the home team Charlotte Hornets (CHA) and the away team Toronto Raptors (TOR) play each other, the testing set will have the summary statistics of CHA vs the summary statistics of TOR. The testing set will also be selected for the best variables from Step 3.
Create a control model.
Train the training set to different model methods, and then implement the newly created models onto the testing set.
Find the model resulting in the lowest mean absolute error (MAE). 

This is just a basic summary of how the process will be performed. Explanations, justifications, and defense of each step will be further explained in the following.

### Creating the Summary Table
The purpose of the summary table is that it will act or pretend to be the box score statistics for future games. There are many advantages and disadvantages to using this strategy. Firstly, some advantages are that using the averages of the NBA teams' stats in place of the predictor variables can create an accurate simulation of how a team will fare against another. The summary table also will consider great and bad teams, as great teams often will have winning averages, and bad teams will have losing averages. One drawback is that I will not be able to predict unusual games, such as lower-ranked teams beating higher-ranked teams. The summary table itself is not a model by any means, it is just the representation of what possibly the box score statistics could be in the future. The model will use the summarized box score statistics to predict the desired outcome, which is the spread.

Below is the code of how I summarized across the dataset using the NBA team name as the grouped set.

```r
MEAN_DF <- NBA_SEASON_GAMES_MEAN %>%
  group_by(slugTeam) %>%
  summarise(across(where(is.numeric), mean, na.rm = TRUE))
```

The following table shows the first 5 rows and first 5 variables of the summary table, which includes the names of each NBA team and the respective average in its season. The actual table contains 30 NBA teams as rows, and 28 variables, including the Team variable.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/1p1.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Table 2.1 Preview of the Summary Table.
</div>

### Preparing NBA Season Games Dataset for Training
Back in Section 1.1, I use that data set to set up the training set. Displayed below are the actual outcomes that happened in 5 NBA games. “.x” denotes the home box score statistics, and “.y” denotes the away box score statistics. In the actual data frame, there are many, many more variables, but I wanted to show how each variable is paired between home and away.

In this step, I create the response variable, Spread, which is the difference between the points the home team earned and the points the away team earned.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/1p2.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Table 2.2 Preview of the Training Set for Predicting Spread.
</div>

### Finding the Best Variables from the Training Set
I performed a stepAIC in Table 2.2 to find what the best variables were that explained the Spread variable. The variables vary year by year, but in 2023, the best variables are shown below:

```r
get_model_call(Best_Spread) 
## glm(formula = Spread ~ plyrESPNavg.x + fg2mTeam.y +  trebTeam.y + pctFG3Team.y + ptsTeam.x + fgmTeam.x + tovTeam.y + stlTeam.x, family = "gaussian", data = NBA_SEASON_GAMES_COMBINED_LESS) 
```

As shown, the best variables in 2023 that explained Spread were the ESPN player rating stat, points scored, and field goals made by the home team. The other variables are the 2-point field goals made, total rebound, 3-point field goal percentage, and turnover stat for the away team.

The best variables will be used in a future step.

### Preparing for Model Creation
#### Training Set
The training set will be exactly like Table 2.2, however, I apply the step I did in Section 2.3 here by selecting the variables in this dataframe that the stepAIC outputs.

#### Testing Set 
The testing set will be games played in April. I chose April because that is the month the NBA Regular Season ends, and it would match what I am trying to accomplish by predicting the NBA 2024 April games starting on the 9th.

The testing set is formatted identically to the training set, however, in place of the box score statistics will be the summary statistics for each team. Recall that each row of the training set is one single game. The testing set will also be one single game, but it will reflect the state of future games using that team's average box score statistic.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/1p3.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Table 2.3 Preview of the Testing Set for Predicting Spread.
</div>

Table 2.3 shows that the variables that hold the box score data are now replaced with the summary of each team. Observe the Boston Celtics (BOS) in the table above. In row 2, since they are away, they are the “.y” variable. BOS has an average of 42.20 fgmTeam.y, or around 42 field goals made per game in 2023. In row 3, they are the home team, which means fgmTeam.x should also be 42.20. Indeed, as in row 3, column 3, the home team, BOS, has a field goal-made average of 42.20. 

However, the testing set is not yet completed. The last adjustment is selecting the best variables generated in Section 2.3. This should result in both the training and testing set to have the same variables, as they are now ready for model testing.

### Constructing the Control “Model”
The purpose of the control model is to give a baseline for the worst-case scenario. The worst case scenario in the case is applying no model to the data, and simply predicting what the score of future games will be using the average ptsTeam variable I calculated in Section 2.1 for each team.  

Below are the first five rows of the control data frame. Actual_Spread is simply the spread of what actually happened in the game, for instance, in row 1, BOS vs. ATL had an Actual_Spread of 6, meaning BOS beat ATL by 6 points. The variable controlSpread is the difference between ptsTeam.x and ptsTeam.y. The variable Diff_Control is the absolute difference between Actual_Spread and controlSpread.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/1p4.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Table 2.4 Preview of the Control Dataframe for Predicting Spread.
</div>

Later, I will calculate the MAE of this data frame and compare it to the models I will create.

### Model Creation
For the next three models, I will use the training and testing set produced in Section 2.4 for all three methods. The use of the training and testing set are all similarly interchangeable, making it easy and efficient to test multiple methods.

#### Linear Regression
The group chose linear regression as one of the model methods because it is simple and interpretable. Linear regression is also less prone to overfitting, as the dataset has many observations. Linear regression is simply the relationship between the predictor and response variable, where for every unit increase in a predictor, the response is said to increase or decrease by a certain amount.

Below is an example of how I ran the code for linear regression. The response variable is Spread, and the predictor variables are the best variables created from the stepAIC in Section 2.3.

```r
train_set <- NBA_SEASON_GAMES_COMBINED_LESS
test_set <- final_data_frame_fortesting

# Fitting the linear regression model on the training set
linear_model <- lm(Spread ~ ptsTeam.y + ptsTeam.x + astTeam.x + fg2mTeam.y + 
    fg2aTeam.y + ESPNdiff + blkTeam.y + pctFG3Team.x, data = train_set)

# Making predictions on the testing set, and storing them
Spread_Predictions <- predict(linear_model, newdata = test_set)
```

#### Random Forest
The Random Forest method is a machine learning technique that, unlike the linear regression method, can take non-linear relationships. As the name states, Random Forest uses ‘decision trees’ to create an accurate model. Decision trees are basically a series of questions the model asks the dataset, and the answers to the questions lead to the best possible algorithm to predict an outcome. Random Forests could be useful for predicting Spread, as the data contains lots of variables and observations in which the Random Forest could detect unseen relationships. 

Below is how I set up the model for Random Forest. Notice how I selected certain variables for the training and testing data in the code chunk. This part results from stepAIC from 2.3, as I select the best variables. The features and target variable refer to the predictors and response variable, Spread, in the training set respectively.

```r
train_data <- NBA_SEASON_GAMES_COMBINED_LESS %>% dplyr::select(Spread, ESPNdiff, trebTeam.x, fgmTeam.x, fg3mTeam.x, Rank.y, pctFTTeam.x, pctFG3Team.y, stlTeam.y)

test_data <- final_data_frame_fortesting %>% dplyr::select(Spread, ESPNdiff, trebTeam.x, fgmTeam.x, fg3mTeam.x, Rank.y, pctFTTeam.x, pctFG3Team.y, stlTeam.y)

# Specifying features and target variable
features <- setdiff(names(train_data), "Spread")
target <- "Spread"

# Training the Random Forest model
rf_Spread <- randomForest(x = train_data[features], y = train_data[[target]], importance = TRUE)
Spread_Predictions <- predict(rf_Spread, newdata = test_data[features])
```

#### Gradient Boosting Machines
Gradient Boosting Machine (GBM) is another machine learning method with a methodology similar to Random Forest but a different approach. GBM starts by using one decision tree, then slowly adds more. Each time the machine adds more, it makes micro-adjustments and error corrections from the previous decision trees. The GBM attempts to minimize the errors to improve the function. The strength of GBM is that it can improve on its own weaknesses, making it very versatile.

Below is the code of what the model looks like. Again, I only select the best variables for the training and testing set. 

```r
train_data <- NBA_SEASON_GAMES_COMBINED_LESS %>% dplyr::select(Spread, ESPNdiff, trebTeam.x, fgmTeam.x, fg3mTeam.x, Rank.y, pctFTTeam.x, pctFG3Team.y, stlTeam.y)

test_data <- final_data_frame_fortesting %>% dplyr::select(Spread, ESPNdiff, trebTeam.x, fgmTeam.x, fg3mTeam.x, Rank.y, pctFTTeam.x, pctFG3Team.y, stlTeam.y)
dtrain <- xgb.DMatrix(data = as.matrix(train_data[,-which(names(train_data) == "Spread")]), label = train_data$Spread)
dtest <- xgb.DMatrix(data = as.matrix(test_data[,-which(names(test_data) == "Spread")]), label = test_data$Spread)

# Set the parameters for xgboost
params <- list(
  booster = "gbtree",
  objective = "reg:squarederror",
  eta = 0.1,
  gamma = 0,
  max_depth = 6,
  min_child_weight = 1,
  subsample = 0.5,
  colsample_bytree = 0.5
)

# Train the model
nrounds <- 100 # number of boosting rounds
modelgm <- xgb.train(params = params, data = dtrain, nrounds = nrounds)

# Predicting
Spread_Predictions <- predict(modelgm, dtest)
```

### Choosing a Final Model
I gathered the results by running the models through the years 2012-2023 (excluding 2020 because of COVID-19) and finding the MAE of each model.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/1p5.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Table 2.5 MAE Results for Predicting Spread.
</div>

Next, I took the averages of each method and found the one with the lowest MAE. Notice how the Control and Linear Regression MAE both have identical MAE. While I used different methods to calculate both, I discovered that the process of doing each one was identical in the result.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/1p6.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Table 2.6 Average  MAE  of Different Spread Predicting Methods.
</div>

The best method to calculate Spread is by Random Forest, which gives us an MAE of 11.08. 