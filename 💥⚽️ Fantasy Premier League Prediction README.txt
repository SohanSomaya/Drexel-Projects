# ✨️⚽️ Fantasy Premier League Prediction README

## ℹ️ Overview
This dataset consists of Premier League fixtures and scores, as well as calculated descriptive and predictive metrics for team performance.

## 📁 Dataset
This dataset is a **single CSV** file. It contains fixtures, scores, and expected goals (a measure of attacking opportunity quality) for the 2023/24 and 2024/25 Premier League seasons. This data is scraped from FBRef, a website that aggregates football statistics. The other source of data is head-to-head betting odds from OddsAPI, a sports betting API that provides odds from bookmakers around the world.

Head-to-head odds are the potential payout in 3 scenarios: home team winning, away team winning, and a draw. For example, odds of 4.2 mean that a bet of $1 pays out $4.2. We applied statistical methods (Poisson distribution) to estimate the number of goals most likely to be scored by each team. We also applied statistical methods to assess historical form for each team using an exponential moving average. This method averages the performance of a team's last 10 games but weights more recent games more heavily.

Our approach was guided by this resource:
[Poisson Distribution in Soccer Betting](https://www.pinnacle.com/betting-resources/en/soccer/poisson-distribution-predict-the-score-in-soccer-betting/md62mlxumkmxz6a8)

To learn more about how expected goals are calculated, please reference:
[Expected Goals (xG) Explained](https://statsbomb.com/soccer-metrics/expected-goals-xg-explained/)

## 📊 Column Descriptions

| Column Name          | Column Description |
|----------------------|-------------------|
| **Season**          | Season match was played in |
| **Date**            | Date of match |
| **Home**            | Home Team |
| **Score**           | Results of the match |
| **Away**            | Away Team |
| **home_CS%**        | Likelihood the away team scores zero goals. Calculated from the odds data using the Poisson distribution. |
| **odds_home_xG**    | Mean number of goals likely to be scored by the home team. Calculated from the odds data using the Poisson distribution. |
| **score_home_ewma** | Average goals scored per game by the home team over the last 10 games. Exponential moving weighted average that weights recent games more heavily. |
| **xG_home_ewma**    | Average xG per game by the home team over the last 10 games. Exponential moving weighted average that weights recent games more heavily. |
| **ewma_diff_home_xG** | Difference between the moving average for goals scored and xG for the home team. Positive values indicate overperformance, negative values indicate underperformance. |
| **away_CS%**        | Likelihood the home team scores zero goals. Calculated from the odds data using the Poisson distribution. |
| **odds_away_xG**    | Mean number of goals likely to be scored by the away team. Calculated from the odds data using the Poisson distribution. |
| **score_away_ewma** | Average goals scored per game by the away team over the last 10 games. Exponential moving weighted average that weights recent games more heavily. |
| **xG_away_ewma**    | Average xG per game by the away team over the last 10 games. Exponential moving weighted average that weights recent games more heavily. |
| **ewma_diff_away_xG** | Difference between the moving average for goals scored and xG for the away team. Positive values indicate overperformance, negative values indicate underperformance. |
| **score_home**      | Goals scored by the home team |
| **xG_home**        | Expected number of goals scored by the home team based on the quality of chances created. |
| **xG_home_diff**   | Difference between goals scored and xG for the home team. Positive values indicate overperformance, negative values indicate underperformance. |
| **score_away**      | Goals scored by the away team |
| **xG_away**        | Expected number of goals scored by the away team based on the quality of chances created. |
| **xG_away_diff**   | Difference between goals scored and xG for the away team. Positive values indicate overperformance, negative values indicate underperformance. |

## 🧮 Selected Summary Statistics
The dataset consists of information from the 2023/24 and 2024/25 Premier League seasons and consists of 759 games. The average home xG is **1.7**, and the average away xG is **1.4**. The same is true for actual goals scored, showing how strongly correlated the metric is with team performance.

## 🤦‍♂️ Challenges in Data Collection
We initially intended to obtain the Premier League statistics using the FBRef API but encountered server-side issues. As an alternative, we used `read_html()`, which met our needs but introduced formatting issues and NaN values due to unusual whitespaces and empty cells, requiring data cleaning.

Another challenge was that the free version of the Odds API does not provide historical betting odds and only provides odds for two weeks into the future. As a result, only two weeks of games in the table have associated betting odds. However, we designed our dataframe to retain historical odds when calling the API for new games.

A further challenge with odds data is that if the API is called while a game is underway, the odds may be heavily skewed. For example, if a team is losing heavily with only a minute left, the odds would not reflect pre-match expectations. Therefore, it is recommended to run our script before games begin.

## ⚽️ Use-Cases
Fantasy Premier League (FPL) is the official fantasy game for England’s top-flight soccer league. Each week, managers select 11 Premier League players as their fantasy team. The objective is to select top-performing players to maximize points. Players score points based on goals, assists, and clean sheets (no goals conceded).

Analyzing fixtures to determine which games are likely to be high-scoring and/or contain clean sheets is essential for success in FPL. Our dataset enables this analysis by providing match-by-match performance data and predictions for teams.

