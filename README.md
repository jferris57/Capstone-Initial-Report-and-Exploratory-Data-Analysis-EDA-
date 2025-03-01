# Capstone Initial Report and Exploratory Data Analysis
This project is an exploratory data analysis (EDA) and an initial report for my capstone project.

## Research Question: Can we predict how many fantasy points an NFL player will score?
In the world of fantasy sports and sports betting, gaining an edge is everything. Whether you're competing in a fantasy league with friends for bragging rights (and maybe a bit of cash) or analyzing odds at a professional sportsbook, knowing how players will perform is crucial.

Sportsbooks rely on highly accurate models and projections to turn a profit, while bettors and fantasy players use data-driven insights to make informed decisions. My analysis aims to help fantasy football players and sports bettors navigate the unpredictable and chaotic football season with more confidence.

The data I will be using in this project is from Kaggle and can be found here: https://www.kaggle.com/datasets/philiphyde1/nfl-stats-1999-2022

Included in this data is various stats for players on a yearly basis. This includes stats like receptions, targets, receiving yards, rushing yards, yards after catch, etc.

The Jupyter Notebook containing my work can be found [here](https://github.com/jferris57/Capstone-Initial-Report-and-Exploratory-Data-Analysis-EDA-/blob/main/Capstone_Assignment_Initial_Report_and_Exploratory_Data_Analysis_(EDA).ipynb) .

## Understanding The Data
Initially looking at our data, there are a ton of features (195). There's all sorts of information like receiving yards, rush yards, injuries, interceptions, career yards after catch, height, weight, etc. I want to reduce this number and select a decent amount of useful features that are correlated with our target variable. The provided Jupyter Notebook lists every column in the dataset.

The dataset provides samples from seasons 2012-2023.

## Data Preparation

I removed the 'game type' feature as there was only one value. I identified our target as the 'fantasy points ppr' feature. I also checked the null count and converted this to a percentage of all the datapoints.

The 'vacated' columns consisted of a lot of missing data (6.33% of the dataset) and there was 1 value missing from the 'college' column.

I removed the one row containing missing college info and then checked the correlation between the 'vacated' features and our target. All of these features had low correlation ranging between -0.009 to -0.05 so I removed them entirely.

I then noticed there are some columns with weird values like 'inf' and '-inf' as well as some columns with very large numbers in the negatives. I decided to remove the data that had inf and -inf. 

Some of the columns containing large negative numbers were 'passer rating' and 'delta passer rating'. Passer ratings in the NFL are on a scale from 0 to 158.3. For this reason, I replaced any negative values with 0. As for the other columns with very negative values, I set them to values at the 5th percentile.

After this, the data was clean of missing values and very large negative values.

## Visualizations
Now that our data is clean, I wanted to investigate some of the data visually.

Here is the distribution of fantasy points over the whole dataset. This data is heavily skewed, but this is expected as we usually have few 'elite' fantasy players.

![fantasy-points](https://github.com/user-attachments/assets/9acaa2f9-6740-49e4-b89a-589b9e060be8)

Here is the same data in boxplot form:

![boxplot-fantasy-points](https://github.com/user-attachments/assets/7e7df48f-99c8-4065-80f2-f53888d32a0a)

I then looked at the fantasy points scored by team and by position. I wanted to see if where players are playing and what position they are playing helps determine more or less fantasy output.


![team-points](https://github.com/user-attachments/assets/f5aa0279-e2c0-41ea-a203-c0e707cf2cee)


![position-points](https://github.com/user-attachments/assets/85e7322d-b1aa-4f42-98f3-0004de75fbce)

The team points dont look much different from each other, we expect better teams to allow more fantasy points than worse ones. The positional fantasy data was expected as usually QB's will have a high fantasy point output followed by WR's and RB's. It looks like running backs have a slightly lower mean than wide receivers and I believe this is expected because usually there are more viable wide receivers in fantasy than running backs.

I then looked at receiving yards by position, rushing yards by position, and total yards by position.

![receiving](https://github.com/user-attachments/assets/31911f36-65f8-48d8-b4cf-60d09a28cbcd)

![rushing](https://github.com/user-attachments/assets/0dce28be-8608-4962-94b8-2c4bc4ff259d)

![total-points](https://github.com/user-attachments/assets/63fd4f1f-fac6-43d4-863e-f8a67b59a718)

I then looked at points per game by position with QB's leading, followed by WR's then RB's and finally TE's.

![PPG](https://github.com/user-attachments/assets/0eecd211-8475-44a3-804e-adfb8f8348cd)

Finally, I looked at the fantasy point output trend for AJ Brown and CeeDee Lamb. We can see that barring any injuries, fantasy point output trends upward as players become more experienced and play more seasons.

![brown-trend](https://github.com/user-attachments/assets/b1ba53d4-a106-44fd-afde-86732d7c8e1d)

![lamb-trend](https://github.com/user-attachments/assets/0005494b-a9c9-40af-9b05-a18efa3d6243)

I would like to analyze the fantasy point trends for other players over an entire career as we should expect some decline in fantasy points as they become a certain age.

## Engineering Features

I created a correlation matrix of the numerical features filtered by our target. I selected only the features with a correlation greater than 0.4 or less than -0.4 so we could use good features that were predictive of our target variable. The attached Jupyter notebook will list all of these correlation scores.

I then realized I did not want to keep player names, player ID's, or colleges. Player names or ID's are just identifiers and I dont want to One Hot Encode them and use them to train a model. As for colleges, there was a very large amount of them and since I don't think they very strongly can predict fantasy points, I removed those too. However, I did create a separate dataframe with these values to use later so that we can present our findings.

The last remaining categorical variables were team and position. There are 32 teams so I did not want to One Hot Encode these and create a large amount of new features. Instead, I analyzed their impact to fantasy point output by grouping our dataset by team and taking the mean fantasy point output. This resulted in a numerical value that could determine which teams created more fantasy point output on average than others. I created a new column of these values and removed the 'team' column. 

The last categorical feature is position, and I will One Hot Encode these values for model training.

## Modeling
### Baseline Model
The last step of this initial data analysis was to train a baseline model. I chose a default Linear Regression model as a baseline. I fear that this model is overfitting, as our R2 score and MSE scores are both extremely high. 

![image](https://github.com/user-attachments/assets/be2abd66-8d4f-460f-a5a0-21b982187c8c)

## Next Steps
For the next portion of my Capstone project, I will train additional models such as Ridge, Lasso, SVR, Transformed Target Regressor, etc. I will use Voting Regressor to train an ensemble model. I will test if regularized models, cross validation, and hyperparameter tuning/grid searching can help reduce overfitting.








