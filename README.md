# League of Legends Classifier: Who will win?
### Vishwak Pabba and Manav Jairam

## **Framing the Problem**
League of Legends is an online, multiplayer game where players in teams of 5 battle each other in Summoner's Rift, a massive arena that has an expansive environment and monsters. Players fight against each other to destroy the enemy by toppling turrets and non-playable minions, slowly advancing through 1 of 3 lanes (top, bottom, middle) towards their enemies' nexus crystal. When a team destroys their enemy nexus crystal located in their base, they ultimatly win the game.

The dataset we used for analysis was found on https://oracleselixir.com/tools/downloads, and contained competitive match data from 2022. The dataset included information about the team composition in each competitive match, an individuals game statistics in each respective match, and match details (`date`, `split`, `game ID`, etc.).

In this project, we are choosing a classification problem for our League of Legends dataset. Our problem statement is to predict whether a team will win or lose a game. This is a binary classification, as we are only predicting two values - a win or a loss, and we are not predicting a range. Our response variable is the `result` column in the dataset. We picked this as it clearly shows whether a team won or lost a game. One adjustment we made was to only use this result for `teams` instead of individual players. To evaluate our model, we are using F1 - score as our metric. Accuracy can be misleading when the dataset has imbalanced class distribution, meaning one class has significantly more instances than the others. In such cases, a model can achieve high accuracy by simply predicting the majority class for every instance. The F1-Score considers both precision and recall, which provides a balanced evaluation by taking into account true positives, false positives, and false negatives. Essentially, we prefer F1 as it is a more robust metric when dealing with a potentially imbalanced dataset.

## **Baseline Model**
Our baseline model is a logistic regression model. It is trained using two features from our dataset, namely `kills` and `deaths`. We purposefully dropped the individual rows from the model and used only the team rows, since those values include the total kills and deaths for each team. We used 80 percent of the data to train the model, and 20 percent to test. Both the features we used are quantitative. The only form of encoding we used on the basic model was StandardScaler for both the quantitative features. As mentioned above, we chose the F1-Score metric to predict values in our model. We got ~0.957 as our score - we think that this is a good starting point and shows that we have some of the right features, but we can definitely improve upon the model. For performance on unseen data and generalizability, we’re getting better results on the test dataset, which shows that the model works equally well with new data from the same data generating process. 

## **Final Model**
For the final model, we chose a DecisionTreeClassifier. We chose this model as it can handle both categorical and numerical values, and is robust to outliers. 

For our final model, we used the `side`, `kills`, `deaths` features from the dataset. We also used columns 40-69 in the dataset. Columns 40-60 represent the number of special monster kills that a team had. These monsters would provide buffs to the teams damage output and their minions output, which would give them an edge against the other team in getting the victory. Columns 61-69 represent tower metrics - `inhibitors`, `first to three towers`, etc. All these metrics work on a "game momentum" principle - whichever team is able to achieve these metrics and achieve them fast gains momentum and is more likely to win the game. All these features in columns 40-69 are quantitative, with their values representing how many of each were achieved. `kills` and `deaths` are also quantitative, with the only nominal categorical variable being `side`. These reasons are why we believe that the features would improve our model, purely from the perspective of the game and the data - generating process.

In encoding these features, we chose a variety of different methods. For `kills` and `deaths`, we chose to continue with the StandardScaler from the baseline model. For `side`, being a categorical variable, we OneHotEncoded it. For the rest of the features, we used a Binarizer, as we just wanted to see whether they achieved the feature or not. 

In order to choose and tune hyperparameters, we used GridSearchCV. We used 5 folds with this K-Fold Cross-Validation method. Even though we had a lot of features, we felt that GridSearch was the most robust and comprehensive process that we could use to get an improvement in our model performance while also not compromising on generalizability. 

**This is the best hyperparameter combination we got: {'preprocessor__binarizer__threshold': 6, 'tree__criterion': 'entropy', 'tree__max_depth': 10, 'tree__min_samples_split': 30}**

Our model is able to work well on both seen data and unseen data. This can be seen with our model getting an F1-Score ~0.98, which is shown to improve from before. We’re also getting better results on the test dataset, which shows that the model works equally well with new data from the same data generating process.

## **Fairness Analysis**
In our Fairness Analysis, we aimed to assess the fairness of a model by comparing the accuracy of our model in determining which team will win, as we noticed that the blue team generally had a slightly greater accuracy.

**Null Hypothesis**: The classifier's accuracy score is equal for teams starting on the blue side and teams starting on the red side, attributing any differences to chance.

**Alternative Hypothesis**: The classifier's accuracy score is higher for teams starting on the blue side.

To measure parity, we chose accuracy as the evaluation metric, focusing on the model's performance for both red and blue teams rather than the specific types of errors it generated. As a test statistic, we selected the signed difference of group means since we encoded both columns as quantitative variables using One Hot Encoding. By opting for the signed difference, we aimed to determine if the model was fair or biased, by indicating the direction of bias. We set a significance level of 0.05 for this analysis. The result of our permutation testing yielded a p-value of ~0.52, indicating that we failed to reject the null hypothesis, suggesting that the classifier's accuracy score was not higher for teams starting on the blue side.
