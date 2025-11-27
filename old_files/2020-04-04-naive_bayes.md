---
layout: post
title: Naive Bayes
date: 2020-04-04
description: 
tags: ML 
categories: study
---

#### Naive Bayes 

Naive Bayes algorithm is a supervised learning method using Bayes rule with conditional independence assumption over data attributes. 

First, Bayes rule says that $$ P(Y \vert X) $$ can be expressed by $$ P(X \vert Y) $$ and $$ P(Y) $$. It is particularly useful when $$ P(Y \vert X) $$ cannot be computed directly. 

$$
P(Y_k|X) = \dfrac{P(X|Y_k)P(Y_k)}{\sum_j P(X|Y_j)P(Y_j)}
$$

#### What is the "naive" part of Naive Bayes algorithm

Naive refers to our assumption that if we have n attributes, $$ X = <X_1, X_2, ..., X_n> $$, then, these attributes are **conditionally independent** from one another. 

When we say "$$ X_1 $$ is conditionally independent from $$ X_2 $$ given $$ Y $$", we write it as below . 

$$
P(X_1|X_2,Y) = P(X_1|Y) 
$$

This means that $$ X_1 $$ and $$ X_2 $$ are conditionally independent. It is a critical assumption to make our algorithm tractable. 

Here is a perfect example from the CMU lecture. To predict thunder, it doesn't matter if it's raining or not, when we see lightning. Thunder is conditionally independent from rain, when lightning is observed. 

$$
P(thunder | rain, lightning) = P(thunder|lightning)
$$

#### Why do we need to be naive?

What is the reason for talking about conditional independence? Let's say we are training a Bayes classifier without this assumption. 

We need to have some training data to predict the parameters for $$ P(Y) $$ and $$ P(X|Y) $$.
Say $$ x_i $$ is a $$ n $$-dimensional vector representing $ n $ attributes. Below is a conditional joint probability, $$ P(X_1, X_2, ..., X_n \vert Y) $$.

$$ 
\theta_{ij} \equiv P(X= x_i|Y=y_j)
$$

Each of $$ x_i $$ can take $$ 2^n $$ possible values. To be exact, we need $$ 2^n -1 $$, since summing over all $$ x_i $$'s is 1, when we fix $$ Y=1 $$. Because we have to estimate for both $$ Y = 1 $$ and $$ Y = 0 $$, we need $$ 2 * (2^n -1) $$ in total. 
So then, what if we have 30 features? The number of parameters becomes over 1 billion. In other words, we need more than 1 billion data exampes to represent all joint probabilities... 

Conditional independence to the rescue!  
We will only need $$ 2*n $$ parameters, because now with conditional independence, we have 

$$ 
P(X_1,X_2,...X_n|Y) = \prod_i{P(X_i|Y)} 
$$ 

We no longer need to estimate the joint probability; we just need to figure out $$ P(X_i \vert Y) $$.  
The number of computation has reduced from $$ 2*(2^n-1) $$ to $$ 2*n $$ (given 30 features, 1 billiont to 60).

#### Extending to discrete values (multinomial) 
Now instead of each $$ X_i $$ taking 0 or 1, let's say it can take $$ j $$ different discrete values. Same for $$ Y $$, now it can take $$ K $$ different discrete values. It's a simple extension to the above.

We need to estimate the probability that $$ X_i $$ will take $$ j $$-th value, using MLE. 

$$ 
\theta_{ijk} =  P(X= x_{ij}|Y=y_k) = \dfrac{\# D\{X=x_{ij} \wedge Y=y_k\}}{\#D\{Y=y_k\}} 
$$   


Or MAP  

$$
\theta_{ijk} =  P(X= x_{ij}| Y=y_k) = \dfrac{\# D\{X=x_{ij} \wedge Y=y_k\} + h}{\#D\{Y=y_k\} + hJ} 
$$  

where $$ h $$ is number of halluciating examples.

Also we need to estimate the prior probability of Y, using MLE. 

$$ 
\pi_k = P(Y=y_k) =  \dfrac{\# D\{Y=y_k\}}{\#D} 
$$ 

or MAP.

$$ 
\pi_k = P(Y=y_k) =  \dfrac{\# D\{Y=y_k\} + h }{\#D+hK} 
$$ 

MAP can be preferred over MLE, when there is limited data, so that we can avoid zero probabilities. 

#### Extending to continuous values 
When the input values are continuous, we will use a probability distribution to estimate $$ P(X \vert Y) $$ and $$ P(Y) $$, instead of counting the number of observations in the data.    
Simplest way is to use the gaussian distribution. Our task becomes estimating the mean and the variance of each attribute $$ X_i $$ given that $$ Y=y_k $$, and of the prior $$ Y $$. I coded a simple classifier for sad music classification (described below).   

In real applications, additional assumptions can be added to reduce the number of parameters to estimate. For example, if we observe a common noise that is independent from the training objective, we can assume the variance of all the attributes and/or the priors are the same. 


#### Pseudo code for training a naive bayes classifier 
For each value of $$ Y=y_k $$ :      
* estimate prior $$ P(Y=y_k) $$      
* For each value of $$ x_{ij} $$  :      
  * estimate $$ P(X=x_{ij} \vert Y=y_k) $$       

#### Finally! Sad music classifier with Gaussian Naive Bayes
I implemented a gaussian naive bayes classifier for sad music using audio feaures provided by [Spotify Web API](https://developer.spotify.com/documentation/web-api/). To make the data, I simply grabbed 100 songs each from a "happy" playlist (["Happy Hits!"](https://open.spotify.com/playlist/37i9dQZF1DXdPec7aLTmlC?si=_bMVBft5REu2ge_e4bIfbgand)) and a "sad" playlist (["Sad Songs"](https://open.spotify.com/playlist/54ozEbxQMa0OeozoSoRvcL?si=P2ODWYZ1S-Wo_IkFaSX8_A)).  

Specifically, I chose 5 attributes, 'danceability', 'tempo', 'energy', 'valence' and 'loudness'. These values are obtained using [spotipy](https://github.com/plamere/spotipy), which is a python wrapper for Spotify Web API.  
Below is a data distribution for each attribute, where blue indicates sad music and red indicates happy music. 

![distribution]({{"/assets/dist.png"}}){:width="800px"}

For each of the attributes, I calculate the mean and std for the gaussian distribution.   
Here are few values :  
* $$ P(X = Danceability \vert Y = Sad) $$ : $$ \mu $$ = 0.5505, $$ \sigma  $$ = 0.1013
* $$ P(X = Danceability \vert Y = Happy) $$ :  $$ \mu $$ = 0.6923, $$ \sigma $$ = 0.1233   
* $$ P(X = Valence \vert Y = Sad) $$  : $$ \mu $$ = 0.3515, $$ \sigma $$ = 0.1908
* $$ P(X = Valence \vert Y = Happy) $$ : $$ \mu $$ = 0.5794, $$ \sigma $$ = 0.1815

From this, we can calulate the probability of attribute $ X_i $ taking a certain value by using a normal probability density function.  

$$ 
p(x) = \dfrac{1}{\sigma \sqrt{2 \pi}} e^{-\dfrac{(x-\mu)^2}{2\sigma^2}} 
$$    



Prior probability is $$ P(Y=Sad) = 0.5 $$ and $$ P(Y=Happy) = 0.5 $$, since I used 30 songs for each class.  

Now, all we need to do when we have a new data observation $$ X $$ is to compute $$ P(Y \vert X) $$ using Bayes rule.  

$$ 
P(Y=Sad | X = [X_{danceability}, X_{tempo}, ...]) = P(Y=Sad) * gaussian\_prob(danceability\_stats, X_{danceability}) * gaussian\_prob(tempo\_stats, X_{tempo}) * ... 

$$ 


$$ 
P(Y=Happy | X = [X_{danceability}, X_{tempo}, ...]) = P(Y=Happy) * gaussian\_prob(danceability\_stats, X_{danceability}) * gaussian\_prob(tempo\_stats, X_{tempo}) * ... 
$$ 

From this two estimated probabilities, we take the larger value and say whether the given song is sad or happy. Naive bayes turns out to be pretty good. This yields around 93 % accuracy! A random chance is 50%. I have uploaded the code and data, so if anyone comes across this post and wants to play around, it is [here on my github](https://github.com/kyungyunlee/ml_python/tree/master/sad_music_naive_bayes). 

Coming up next is maybe logistic or linear regression. 

#### Reference 
* CMU Machine learning 2015 course by Tom Mitchell 
* [https://machinelearningmastery.com/naive-bayes-classifier-scratch-python/](https://machinelearningmastery.com/naive-bayes-classifier-scratch-python/)  
