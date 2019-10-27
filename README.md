# Drugs.-Predict-drug-consumption-using-a-set-of-demographic-data-and-five-personality-measurements

**Risk evauation model. Why?**
The data set included information on the consumption of 18 central nervous system psychoactive drugs: alcohol, amphetamines, amyl nitrite, benzodiazepines, cannabis, chocolate, cocaine, caffeine, crack, ecstasy, heroin, ketamine, legal highs, LSD, methadone, mushrooms, nicotine, and VSA (output attributes). There were limitations of this study since the collected sample was biased with respect to the general population, but it remained useful for risk evaluation. Therefore, for me it seems like the reasonable purpose of this ML modelling could be testing if predicting drug consumption was possible and to identify the most informative attributes using data mining methods.

**Selection a reasonable binary target. Motivation.**
I have analyzed an article [1] that says that in most of the databases the classes of users and non-users for most of the individual drugs are imbalanced. But the idea of merging the users of all drugs in one class "drug users" does not seem to be the best solution because of physiological, psychological and cultural differences between usage of different drugs. Instead, I have decided to use correlation pleiades for the analysis of drug usage as a solution to the class imbalance problem because for all three pleiades the classes of users and non-users are better balanced. The article [1] demonstrated that there are three groups of drugs with strongly correlated consumption. That is, the drug usage has a "modular structure". The idea to merge correlated attributes into "modules" is popular in biology. The modules are called the "correlation pleiades". And I have selected a couple of binary targets to analyze & compare the performance of ML models, based on "correlation pleiades":
The Heroin pleiad (heroinPl) includes crack, cocaine, methadone, and heroin.
The Ecstasy pleiad (ecstasyPl) includes amphetamines, cannabis, cocaine, ketamine, LSD, magic mushrooms, legal highs, and ecstasy.
The Benzodiazepines pleiad (benzoPl) includes contains methadone, amphetamines, and cocaine.

**Discriminate participants into groups of users and non-users for binary classification.**
Analysis of the classes of drug users shows that part of the classes are nested: participants which belong to the category ‘Used in last day’ also belong to the categories ‘Used in last week’, ‘Used in last month’, ‘Used in last year’ and ‘Used in last decade’. There are two special categories: ‘Never used’ and ‘Used over a decade ago’ . The article [1] also says that formally only a participant in the class ‘Never used’ can be called a non-user, but it is not a seminal definition because a participant who used a drug more than decade ago cannot be considered a drug user for most applications. And I completely agree with this statement. Hence, there are several possible way to discriminate participants into groups of users and non-users for binary classification:
- Two isolated categories (‘Never used’ and ‘Used over a decade ago’) are placed into the class of 'non-users' and all other 'Used in last day', 'Used in last week', 'Used in last month', 'Used in last year', 'Used in last decade' into the class ‘users’ of drugs. This classification problem is called ‘decade-based’ user/non-user separation.
- The categories ‘Used in last decade’, ‘Used over a decade ago’ and ‘Never used’ are merged to form a group of non-users and all other categories are placed into the group of users. This classification problem is called ‘year-based’.
- The categories ‘Used in last year’, ‘Used in last decade’, ‘Used over a decade ago’ and ‘Never used’ are combined to form a group of non-users and all three other categories are placed into the group of users. This classification problem is called ‘month-based’
- The categories ‘Used in last week’ and ‘Used in last month’ are merged to form a group of users and all other categories are placed into the group of non-users. This classification problem is called ‘week-based’.

I preferred to choose the week-based concept, as I tend to build a prediction model more on the weekly and month relevance of information, as we are with this method more likely to identify really people who use drugs, rather than those who just tried them.
Source of the article [1]: https://arxiv.org/pdf/1506.06297.pdf

**Exploratory analysis**

1) The first hypothesis is:
having analyzed the data, I came to the conclusion that the dataset has not enough data for most of ethnicities and countries to prove the value of this information. It is impossible to completely exclude the possibility that ethnicity and country of residence may be important risk factors, but I believe that they will not help us in any way in the classification and they can be excluded.
2) The second hypothesis is:
in my opinion, such personality traits as sensation seeking, defined by the search for experiences and feelings, that are "varied, novel, complex and intense", and by the readiness to "take physical, social, legal, and financial risks for the sake of such experiences, and openess, which indicates how open-minded a person is (a person with a high level of openness to experience in a personality test enjoys trying new things, they are imaginative, curious, and open-minded), will be strongly correlated with drugs consumption, especially with esctasyPl group, I hardly know about about benzoPl, but about esctasyPl I can say this for 100%, because it's obvious for me, because they make people look for new sensations and be more open to them, and, as you know, it's common that people think that drugs open up a wide range of new sensations, hallucinations, calmness and a sense of well-being and aggravation of the perception of colors and sounds.
3) The third hypothesis is:
in young people 18-30 years of age, the index of sensation seeaking is usually higher, since a large role is played by youthful maximalism and the saying "we live once", therefore at this age they tend to try using drugs.
4) The hypothesis number 4 is:
Also, I can guess that people who left school after 16-18 years old with high score of neurotic state and impulsiveness must be the cause of drug consumption, especially heroinPl i think, because unbalanced and without sufficient conscious control, under the influence of external circumstances or due to emotional experiences, people tend to act rashly and they can easily try a drug, and the experience with drugs can absorb them and they will become addicts.

The results of attempts to prove hypotheses are provided in the notebook.

**Metrics & Validation**
So, I've found out that my dataset is a bit imbalanced on benzoPl target, imbalanced on heroinPl target and balanced on ecstasyPl target.
Source: (https://www.researchgate.net/post/How_to_know_that_our_dataset_is_imbalance)
Selection of evaluation metric also plays a very important role in model selection. Accuracy never helps in imbalanced dataset. Three classification scenarios for four different metrics: accuracy, sensitivity, precision and F1:
1) Accuracy is the fraction of predictions that are true. Although this metric is easy to interpret, high accuracy does not necessarily characterize a good classifier. For instance, it tells us nothing about whether FNs or FPs are more common. If the using drugs is rare, predicting that all the subjects will be negative offers high accuracy but is not useful for our prediction of using this drug. [1]
2) A useful measure for understanding FNs is sensitivity (also called recall or the true positive rate), which is the proportion of known positives that are predicted correctly. However, neither TNs nor FPs affect this metric, and a classifier that simply predicts that all data points are positive has high sensitivity. [1]
Ideally a drug using prediction test should have very low numbers of both FPs and FNs. Individuals who do not have the probability to use a drug should not be given unnecessary treatment or be burdened with the stress of a positive result, and those who do have the probability to use a drug should not be given false optimism about being not a drug user in future.
3) The most popular is the Fβ score, which uses the parameter β to control the balance of recall and precision and is defined as Fβ = (1 + β2)(Precision × Recall)/(β2 × Precision + Recall). As β decreases, precision is given greater weight. With β = 1, we have the commonly used F1 score, which balances recall and precision equally and reduces to the simpler equation 2TP/(2TP + FP + FN). [1]
It should be said that no single metric can distinguish all the strengths and weaknesses of a classifier.[1]
In this particular problem it is extremely important to identify positive values more then negative. Therefore, my decision is to use F1-score, looking on recall-score, because precision plays a big role as well (to be more specifically, to answer 1 not on all objects).
So, I'm going to use F1.
[1] Source : (https://www.nature.com/articles/nmeth.3945)

**Summary**

Firstly, I did a thorough analysis of the dataset, where I found that there were no missing values, determined that the data on drug use and Age, Sex, Education, Country and origin are categorical data, and the data on five personality measurements are numerical, and at the initial stage, looking at the presented set of features, I had different hypotheses. The amount of data presented in the dataset is quite small. The number of Ethnicity represented is extremely small, and the number of respondents is mainly concentrated in one of The ethnicity classes, so the thought immediately arose that this is unlikely to help us in our classification. It was impossible to completely exclude the possibility that ethnicity and country of residence may be important risk factors, but the dataset has not enough data for most of ethnicities and countries to prove the value of this information, so I decided to check this hypothesy. Second, after analyzing articles, I decided to divide drugs on target groups drugs according to their ‘modular structure’ (еhe idea to merge correlated attributes into ‘modules’ is popular in biology). The modules are called the ‘correlation pleiades’: 1) heroin pleiade, 2) ecstasy pleiade, 3) benzo pleiade. Hence, I had 3 targets and dropped columns directly associated with the selected target. After that I did a simpe preprocessing with my data. Then generated hypotheses related to my problem and tried to confirm or deny them using exploratory visualization. Shortly, they were:
1) the dataset has not enough data for most of ethnicities and countries to prove the value of this information such personality traits as sensation seeking and openess will be strongly correlated with drugs consumption, especially with esctasyPl group
2) in young people 18-30 years of age, the index of sensation seeaking is usually higher
3) people who left school after 16-18 years old with high score of neurotic state and impulsiveness must be the cause of drug consumption, especially heroinPl i think
And using different types of plots, I have confirmed all hyphotheses and dropped country and ethnicity columns.
Then, I analyzed the balance of the target variables and came to conlusion that 2 out of 3 target variables are imbalanced. Therefore, It's obvious that in this particular problem it is extremely important to identify positive values more then negative. Therefore, my decision is to use F1-score, looking on recall-score, because precision plays a big role as well (to be more specifically, to answer 1 not on all objects). So, F1 quality metric is suitable for my task.
After, I did one-hot encoding. Why? Because one hot encoding is a process by which categorical variables are converted into a form that could be provided to ML algorithms to do a better job in prediction. For instance (from https://hackernoon.com/what-is-one-hot-encoding-why-and-when-do-you-have-to-use-it-e3c6186d008f which I like), what this form of organization presupposes is VW > Acura > Honda based on the categorical values. Say supposing your model internally calculates average, then accordingly we get, 1+3 = 4/2 =2. This implies that: Average of VW and Honda is Acura. This is definitely a recipe for disaster. This model’s prediction would have a lot of errors. This is why I use one hot encoder to perform “binarization” of the category and include it as a feature to train the model for Age and Education features.
Then, I splitted data into train and test sets using train_test_split, and because my problem is imbalanced, used stratify parameter. Why? Because stratified sampling aims at splitting one data set so that each split are similar with respect to something. In a classification setting where one class isn't much represented in the data set, it is often chosen to ensure that the train and test sets have approximately the same percentage of samples of each target class as the complete set.
After, I used StandardScaler(), because I had planned to work with a linear method, implementing a pipeline to combine the designed preprocessing steps with LogisticRegression. I have found a good combination of parameters, using GridSearchCV and StratifiedKFold() (in cases of imbalanced targets). Repeat the same procedure with Random Forest tree-model, using GridSearchCV to identify the optimal set of parameters and trying different reasonable values of n_estimators and max_depth (I tried huge range of max_depth, but then I read that it's better to use max_depth > 10 in most cases,because higher values easily lead to overfitting). Plotted the dependency of GridSearchCV score on max_depth to check where it is high and where it drops.
And finally, I plotted PR-curves for both linear and tree-based models. Why? Because Precision - Recall curve allows us to understand the possible solutions. What I have learnt: it helps in the interpretation of probabilistic forecast for binary (two-class) classification predictive modeling problems, Precision-Recall curves summarize the trade-off between the true positive rate and the positive predictive value for a predictive model using different probability thresholds. whereas precision-recall curves are appropriate for imbalanced datasets. For a dataset with an equal number of positive and negative cases (like ecstasyPl target and we saw it on graph PR), this is a diagonal line from the top left (0,1) to the bottom right (1,0). Points above this line show skill. In terms of model selection, F1 summarizes model skill for a specific probability threshold, whereas the area under curve summarize the skill of a model across thresholds. This makes precision-recall and a plot of precision vs. recall and summary measures useful tools for heroinPl and benzoPl binary classification problems that have an imbalance in the observations for each class. And I was wondering a lot about "is it possible, that precision at PR curves starts not from 1? on theory he should begin with (0,1) and end (1,0)" and finally I have learnt that yes, it's possible, if the object with the highest probability of being class 1 actually belongs to class 0.
After analyzing the results of F1 metrics and graphs, I came to conclusion that the classification of consumption heroin and benzo pleiads drugs can hardly be solved by using this dataset. A small number of objects, so it is not possible to build a complex model. The problems are really complex, with the set of features that we have, to the problem of a small number of objects is also added that the functions used do not explicitly illustrate the problem, also some of them are not directly related or are not clearly related to the problems or even lack of presented classes in those features. Results are not bad at all in the conditions and problems described above, but we could improve it if there were more data. But also, the problem of ecstasy predictions in which the classes are balanced has been solved more than well, as the metrics and graphs tell us. This problem is not so complex, so it was solved well in opposite with other problems. Also, it should be said that the linear model shows a better result on the test than the nonlinear one, and I have learnt the thing that with given predictions different threshold could give you same precision and recall so sometimes you have flat segments of PR curve. As result non linear model provide us better precision but worse recall. It can be easily explained in each step tree as a part of random forest tryes to split data to identify classes in data better, that exactly precision, that represents the opportunity to distinguish one class from another.
