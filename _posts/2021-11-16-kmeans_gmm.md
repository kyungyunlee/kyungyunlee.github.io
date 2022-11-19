---
layout: post
title: K-means and GMM
description: 
date: 2021-11-16 
tags: ML
categories : study

---

## Unsupervised learning

라벨이 없는 데이터들이 있을때, 그 데이터들을 보고 유의미한 정보를 찾는 것이다. 

Unsupervised learning : clustering = Supervised learning : classification 

## K-means

주어진 데이터를 k개의 클러스터로 묶는 것이 목표이다. 

각 클러스터는 cluster centroid가 기준점이 된다. 그냥 vector space에 한 점이다. 

K-means의 목표는 이 cluster centroid값을 구하고 모든 데이터 포인트를 하나의 클러스터로 라벨링을 해주는 것이다. 

간단히 이 알고리즘을 설명하자면 

1. 일단 데이터에 대한 아무런 사전 정보가 없으므로 cluster centroid들을 랜덤하게 정한다 
2. 각 데이터 포인트를 그것과 가장 가까운 cluster centroid로 분류해준다
3. 각 클러스터마다 자기한테 분류된 데이터 포인트들의 mean값을 구하고 자신의 centroid값을 그걸로 업데이트 해준다. 
4. Converge될때까지 2,3번을 반복한다 

![스크린샷 2021-11-16 오전 12.21.52.png](/Users/kylee/dev/kyungyunlee.github.io/content/posts/K-means an 42af3/스크린샷_2021-11-16_오전_12.21.52.png)

한계점

- hard clustering방법이다
    - 데이터 x가 클러스터 A에 속했다는 것에 대해 1, 0의 바이너리 대답만 줄 수 있다
    - probability, uncertainty에 대한 정보가 없다 → 데이터 x가 클러스터 A에 속한다는 것을 몇프로 확신할 수 있는가? 라는 질문에 답할 수 없다.
- 단순히 눈에 보이는 동그란 클러스터들이 아닌 우리가 쉽게 정의할 수 없는 모양의 클러스터이면 어떻할건가

## Gaussian mixture model

### 목표

라벨이 없는 데이터들이 있는데, 이 데이터들이 어떤 분포들을 가지고 있는지 학습하는 것이다. 

예를 들면, 아래 그림과 같이 데이터들을 가장 잘 나타낼 수 있는 3개의 gaussian distribution의 파라미터 (mean, covariance, weight) 을 학습하는 것이 목표이다. 

즉, 2가지의 큰 결정을 내려야한다. 

1. 어떤 타입의 distribution을 사용할지 
2. Distribution을 대표하는 파라미터들의 값은 어떤걸로 할지

GMM에서는 이름에서 알 수 있듯이 gaussian distribution을 사용한다. 

![1_lTv7e4Cdlp738X_WFZyZHA.png](/Users/kylee/dev/kyungyunlee.github.io/content/posts/K-means an 42af3/1_lTv7e4Cdlp738X_WFZyZHA.png)

### 기본 정의

- Gaussian mixture 이라는 것은 k gaussian 들의 weighted sum 이다
- mean, covariance, weight:  $$ \mu, \Sigma, \pi $$ - 각 gaussian은 3가지의 파라미터들로 나타낸다
- 이때 k개의 weight의 합은 1이다 ⇒ 왜냐하면 weight는 확률이기 때문이다. 어떤 데이터가 있을때 그 데이터가 클러스터 j에 포함될 확률을 나타낸다.
- GMM에서 해야하는 것이 k개의 gaussian의 3개의 파라미터의 최적값을 구하는 것이다.

![스크린샷 2021-11-14 오후 7.50.16.png](/Users/kylee/dev/kyungyunlee.github.io/content/posts/K-means an 42af3/스크린샷_2021-11-14_오후_7.50.16.png)

Side note: 모든 데이터는 (이미지이든 텍스트이든 음악이든) 컴퓨터 입장에서 숫자에 불과하다. 머신러닝에서는 기본적으로 이런 데이터들을 n-dimensional vector로 취급한다. 

### Overview

1단계 : 아무것도 모르는 상태이니까 랜덤하게 파라미터들을 정한다 

![Book Notes-38 복사본.jpg](/Users/kylee/dev/kyungyunlee.github.io/content/posts/K-means an 42af3/Book_Notes-38_복사본.jpg)

2단계: MLE를 계산해서 가장 likely한 distribution으로 라벨을 달아준다 

![Book Notes-38 복사본1.jpg](K-means%20an%2042af3/Book_Notes-38_%E1%84%87%E1%85%A9%E1%86%A8%E1%84%89%E1%85%A1%E1%84%87%E1%85%A9%E1%86%AB1.jpg)

3단계: 각 distribution으로 분류된 데이터들의 mean, std로 distribution의 파라미터들을 업데이트 해준다.

![Book Notes-38.jpg](K-means%20an%2042af3/Book_Notes-38.jpg)

그럼 업데이트 된 새로운 distribution으로 다시 데이터들을 라벨링 해주고 (2단계) 또 파라미터 업뎃해주는 (3단계) 방식을 반복한다. 

이런식으로 iterative하게 업데이트를 하는 방법을 조금 더 general하게 디자인한 것이 EM algorithm이다. 

# Expectation-maximization in GMM

Expectation 단계에서는 likelihood를 계산해주고 (즉 라벨링을 달아주고) maximization 단계에서는 최적의 파라미터들로 업데이트 시켜준다고 보면 된다. 

**먼저 짚고 넘어가야할 개념들** 

* Gaussian density function 

주어진 gaussian이 있을때, x 를 observe할 수 있는 확률을 나타내는 함수 

![스크린샷 2021-11-16 오전 12.02.11.png](K-means%20an%2042af3/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-11-16_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_12.02.11.png)

* Log likelihood

GMM이 잘 학습이 되었다라는 기준은 log likelihood이다. GMM에서는 아래의 log likelihood 가 최대가 되도록 하는 것이 목표이다. (MLE) 

![스크린샷 2021-11-16 오전 12.04.59.png](K-means%20an%2042af3/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-11-16_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_12.04.59.png)

* Latent variable $$ z $$

$$ z $$ 는 one-hot 벡터이고 $$ z_k = 1 $$  또는 $$ 0 $$ 이다. 데이터 x가 k 클러스터에서 포함된다는 것을 나타낼 때 $$ z_k = 1 $$ 이다. 

![스크린샷 2021-11-18 오후 4.38.36.png](K-means%20an%2042af3/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-11-18_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.38.36.png)

* p(z)

클러스터 3개중에 하나의 클러스터를 고른다고 할때 그 확률을 나타낸다. 만약 그 클러스터의 weight가 크다면, 그 클러스터가 선택될 확률이 그만큼 크다는 뜻이다. 

![스크린샷 2021-11-15 오후 11.50.18.png](K-means%20an%2042af3/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-11-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.50.18.png)

* p(x|z)

주어진 클러스터에서 x를 observe할 수 있는 likelihood를 나타내는 수식이다. 이건 gaussian function으로 쉽게 나타낼 수 있다. 

![스크린샷 2021-11-15 오후 11.50.23.png](K-means%20an%2042af3/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-11-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.50.23.png)

* p(x)

앞의 두 식을 이용해서 joint distribution $$ p(x,z) $$ 을 구할 수 있고 이걸 marginalize 함으로써 $$ p(x) $$  를 구할 수 있다. → 데이터 x 를 observe 할 수 있는 확률 

![스크린샷 2021-11-15 오후 11.55.58.png](K-means%20an%2042af3/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-11-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.55.58.png)

### Initialization

랜덤하게 하거나 좀 더 좋은 방식으로 한다. (K-means의 결과를 사용한다던가) 

### E-step
$$ p(z_j=1|x) $$ 를 구하는 과정이다. 

즉, 데이터 x 가 클러스터 j로 label이 될 확률을 구한다 (posterior distribution). 
이건 베이즈 정리를 이용해서 우리가 위에서 구한 $$ p(z), p(x|z) $$ 로 식을 풀어서 값을 계산한다. 

![스크린샷 2021-11-15 오후 11.31.11.png](K-means%20an%2042af3/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-11-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.31.11.png)

### M-step

파라미터들을 업데이트하는 과정이다. 

![스크린샷 2021-11-15 오후 11.31.04.png](K-means%20an%2042af3/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-11-15_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.31.04.png)

끝 -