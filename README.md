# League of Legends Classifier
### Vishwak Pabba and Manav Jairam


## **Introduction**
League of Legends is an online, multiplayer game where players in teams of 5 battle each other in Summoner's Rift, a massive arena that has an expansive environment and monsters. Players fight against each other to destroy the enemy by toppling turrets and non-playable minions, slowly advancing through 1 of 3 lanes (top, bottom, middle) towards their enemies' nexus crystal. When a team destroys their enemy nexus crystal located in their base, they ultimatly win the game.

The dataset we used for analysis was found on https://oracleselixir.com/tools/downloads, and contained competitive match data from 2022. The dataset included information about the team composition in each competitive match, an individuals game statistics in each respective match, and match details (date, split, game ID, etc.). 

Over the course of the project, we cleaned the dataset for the necessary columns that we would use to train various model to classify whether or not a team will win a match. Using a Decision Tree Classifier following hyperparameter testing, we arrived at a model that had an accuracy of ~98%.  

## **Framing the Problem**
In this project, we are choosing a classification problem. Our problem statement is to predict whether a team will win or lose a game. This is a binary classification, as we are only predicting two values - a win or a loss, and we are not predicting a range. Our response variable is the ‘result’ column in the dataset. We picked this as it clearly shows whether a team won or lost a game. One adjustment we made was to only use this result for ‘teams’ instead of individual players. To evaluate our model, we are using F1 - score as our metric. Accuracy can be misleading when the dataset has imbalanced class distribution, meaning one class has significantly more instances than the others. In such cases, a model can achieve high accuracy by simply predicting the majority class for every instance. The F1 score considers both precision and recall, which provides a balanced evaluation by taking into account true positives, false positives, and false negatives. Essentially, we prefer precision as it is a more robust metric when dealing with a potentially imbalanced dataset.

## **Baseline Model**
Our baseline model is a logistic regression model. It is trained using two features from the league of legends dataset, namely ‘kills’ and ‘deaths’. We purposefully dropped the individual rows from the model and used only the team rows, since those values include the total kills and deaths for each team. We used 80 percent of the data to train the model, and 20 percent to test. Both the features we used are quantitative. The only form of encoding we used on the basic model was StandardScaler for both the quantitative features. As mentioned above, we chose the F-1 Score metric to predict values in our model. We got 0.9580364212193191 as our score - we think that this is a good starting point and shows that we have some of the right features, but we can definitely improve upon the model.

## **Final Model**
- For our final model, we used the ‘side’, ‘kills’, ’deaths’, and. We used all the kills features from column 30 - 38, which included features like double kills and triple kills. We used these kills features to determine the ‘momentum’ of the game. By having double, triple and more consecutive kills, the team with those kills can make more of an impact on destroying towers due to not having an enemy champion in the lane as they would be respawning. We also used columns 40-61 in the dataset, which represented the number of special monster kills that a team had. These monsters would provide buffs to the teams damage output and their minions output, which would give them an edge against the other team in getting the victory. All these features are quantitative, with their values representing how many of each were achieved. For example, a 4 on the ‘triplekills’ column would show that the team achieved 4 triplekills. These reasons are why we believe that the features would improve our model, purely from the perspective of the game and the data - generating process.
- In encoding these features, we chose a variety of different methods. For kills and deaths, we chose to continue with the StandardScaler from the baseline model. For the side, being a categorical variable, we OneHotEncoded it. For the rest of the features, we used a Binarizer, as we just wanted to see whether they achieved the feature or not.
- For the final model, we chose a DecisionTreeClassifier. We chose this model as it can handle both categorical and numerical values, and is robust to outliers.
- We chose the following hyper parameters:
- In order to choose and tune hyperparameters, we used GridSearchCV. We used 5 folds with this Cross-Validation method. Even though we had a lot of features, we felt that GridSearch was the most robust and comprehensive process that we could use to get an improvement in our model performance while also not compromising on generalizability.

## **Fairness Analysis**

