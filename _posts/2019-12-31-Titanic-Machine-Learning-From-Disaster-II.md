---

layout: post
title: Titanic:Machine Learning From Disaster-II
subtitle: First Kaggle Competition
tags: [Kaggle, Data Science]

---

This article is Titanic: ML From Disaster part two. The previous post mainly introduces the background and some preliminary work of the competition. We have loaded the data and overviewed the data structure.

### 3.Improve the score
Our goal is finding patterns in **train.csv** that help us predict whether the passengers in **test.csv** survived.

#### Explore a pattern

It might initially feel overwhelming to look for patterns. First, we will start simple. Let's figure out what percent of men and women had survived in this disaster.

```python
male_survived = train_data[(train_data.Sex == 'male') & (train_data.Survived == 1)].shape[0]
male_count = train_data[train_data.Sex == 'male'].shape[0]
rate_men = male_survived/male_count

#rate_men = 0.18890814558058924

female_survived = train_data[(train_data.Sex == 'female') & (train_data.Survived == 1)].shape[0]
female_count = train_data[train_data.Sex == 'female'].shape[0]
rate_women = female_survived/female_count

#rate_women = 0.7420382165605095

```

The code above calculates the percentage of male and female passengers (in train.csv) who survived. From this, you can see that almost 75% of the women on board survived, whereas only 19% of the men lived to tell about it. Since gender seems to be such a reliable indicator of survival, the submission file in gender_submission.csv is not a wrong first guess, and it makes sense that it performed reasonably well!

But only one indicator is rough, and we need multiple factors combined to predict one person is survived or not.

#### Our first machine learning model

We'll build what's known as a random forest model. I will write an article about the random forest model soon. This model is constructed of several "trees" (there are three trees in the picture below, but we'll construct 100!) that will individually consider each passenger's data and vote on whether the individual survived. Then, the random forest model makes a democratic decision: the outcome with the most votes wins!

```python
from sklearn.ensemble import RandomForestClassifier

y = train_data["Survived"]

features = ["Pclass", "Sex", "SibSp", "Parch"]
X = pd.get_dummies(train_data[features])
X_test = pd.get_dummies(test_data[features])

model = RandomForestClassifier(n_estimators=100, max_depth=5, random_state=1)
model.fit(X, y)
predictions = model.predict(X_test)

output = pd.DataFrame({'PassengerId': test_data.PassengerId, 'Survived': predictions})
output.to_csv('my_submission.csv', index=False)
print("Your submission was successfully saved!")
```

### 4.The Result
![](/img/submission_success.png)

In the competition public leaderboard, the score of many people or team is 1.00, I follow the guidelines of other people. The following work for me is improving the accuracy of my submission.
