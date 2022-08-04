# Bussiness Case Exercise

## Introduction

Right now, I have pretty much to do in my work, since I go for 3 weeks vacation from 9/8/2022 to 26/8/2022 and I need to
deliver some products before that period, working overtime.

That's why I did not spend as much time as I wanted for this task. I've decided to dedicate to evenings for the case and
I believe that work is not finished. 
Actually, while working in R&D, work is never being finished.


That's why I will focus in this raport more on what I would do, rather on what I've done.
I'll treat that as a story, going through what I've done with remarks and thoughts:

In following document:
* normal text
* **_comment_**

## Project Structure

Project is documented on the run, so it's readable, but I'm aware it's not the best documentation yet. 


Files are stored as below:

* `data` - raw files storage
  * `temp_data` - processed files storage
* `01_data_analysis_and_preparation.ipynb`
    * main file in which I've explored and preprocessed data
* `02_modeling`
  * file for checking different models
* `result.csv`
  * result of the exercise as described in `Case Instruction.pdf`

## Data - csv file

**_I advice to go through the `01_data_analysis_and_preparation.ipynb` file as I'll describe there pretty everything that
I've done in that notebook._**

First what I did after reading the instruction was loading csv data and explore it.


### preliminary analysis:
  * examine distribution histogram across different columns
    * compare whole dataset and subset of targeted people.


  * from that we have intuition about some features. For example, younger people tend to be targeted more often than older.

  * Males are targeted more often that females

  * daily commute is probably not highly correlated with being targeted

  * relationship status matters. Gym is a place for singles, and you don't often attend after having a child.
  

### data missing examination:
**_For this and following parts, I merged train and test file to get exact same features and avoid different columns in hot encoding.
I split it later._**

* it's polish database so mostly if name is finishing with 'a', with maybe few exceptions, we can fill `sex` missings 
with 'female', otherwise: 'male'. That's what I've done.

* there are many missings in the dataset. For some columns such as `hobby` or `relationship_status` it doesn't matter 
that much because it's going to be hot encoded and we can assume that 'null' values are no relationship status at all,
or no hobby at all.

* for some other, numeric columns, such as `education` or `daily commute`, there are some missings. I've decided to 
handle it later while modeling to see impact of these particular columns in the model. Is it that important to 
predict missings based on other values or just take the average value. Anyway, doesn't matter at this point.

### feature engineering and data preprocessing
* `hobbies` contained list of values which after text cleaning, I've hot encoded for each user.

* I've also hot encoded other columns with classified text values 

* I've created `age` column based on birthdate (youngest person had age=0 but it doesn't matter as it'll be normalized
later)

* I've normalized numeric columns using _min-max normalization_ but as for future work, I'd check different 
normalization algorithms as well.

* I removed not relevant columns
  * these already hot encoded
  * `name` column

## Data - JSON table.

I've loaded JSON table but number of groups was so big (4207 unique groups, 4000 users), it would give me nothing if 
I hot encoded them. I didn't have so much time, but to make the model better I would do:
 1. text processing:
    1. get stopwords out 
    2. get key words using `yake` or smth.
 2. create binary matrix for each word and append it to the user table as a feature.
 3. if there would be a need for making the model better, I'd do it using some Open Source tool or hire some people to 
mark some of the words as connected with sports.
 4. if there would be a need for making the model better, I'd consider making usage of correlation matrix between users 
(smth like collaborative filtering in the Recommender systems.)

    
So, finally I didn't make use of JSON table, that's a cool thing to do for future work.

## Data modeling
My first approach was using Gradient Boosting Classifier because:
- Gradient boosting is known to be good classification method
- it handles missings.


That's the point where everything starts to get more exciting but the time limitations didn't let me do everything as it
should be.

What I've done:
* CV split:
  * model.fit()
  * model.predict()
* train-test split:
  * model.fit()
  * model.predict()


What I would do normally if I had time for that:
* Create Cross Validation subsets:
  * use many subsets and average score by that.
  * use different metrics to check accuracy
    * examine difference between them
* Fit model with:
  * different regularization algorithms included
  * different normalization structures
  * different ways of nan handlings
    * easier - throw out features not relevant for the model
    * easier - using mean, median or some other statistical metrics to fill nans
    * harder - trying to predict values based on other columns
      * ex. predict age basing on person interests
  * compare results taking care of variance, bias

Do above for different algorithms, check which one works the best on CV sets.

and after then:
* `best_model`.predict(`the_most_accurate_data`)

## Summary

It was pretty cool exercise. I wish I had gotten it in different circumstances in which I had more time. I've told about
most of the `TODO`s over the course of the report.

I like the fact
that data is not that great, there are many missings and the preprocessing and feature engineering parts are the ones 
which really matter. As I've mentioned in previous part. I haven't really examined the model as I could.

For one Train - Cross Validation split, the model hit the **86,375%** accuracy which is not bad result in the time spent. 
Result would of course differ if I averaged model over _k_ CV subsets.

If there would be a business need, I'd also like to examine feature impact, to let, for example, target selected people 
with ads, based not only on their age, but also occupation, location, hobbies type (I'd also have to use some `K-means` 
algorithm to get that) etc.

Anyway, thank you for your time reading that.

Piotr Rogula

