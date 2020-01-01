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

