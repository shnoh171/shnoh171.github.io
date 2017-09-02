---
layout: post
title: Deep Learning에 대한 기본적인 소개
published: false
---
// 아직 작성 중인 문서입니다.

* Step 1. 개요 작성 (o)
* Step 2. 글쓰기 (x)

본 포스트는 deep learning을 처음 접하는 학생들과 연구원들이 알아야 하는 배경과 기초 지식들을 공유하기 위해 작성되었습니다. Machine learning에 대한 기반 지식은 필요하지 않으나, 대학 학부 수준의 수학 지식은 가지고 있다고 가정하고 썼습니다. 지금  부터 Deep learning의 정의와 역사를 간단하게 짚은 후, 기본적인 neural network 이론들에 대해 알아 보겠습니다.

## Deep Learning이란 무엇인가

* Deep learning의 정의
  + Deep learning is a class of techniques which allows computational models that are composed of multiple processing layers to learn representations of data with multiple levels of abstraction
  + 결국은 neural network를 꾸미는 advertise term
  + (조사: 누가 deep learning이라는 단어를 유행시켰는가?)
* Neural network
  + Neuron
  + Network
  + 무엇이 좋은가? - 복잡한 함수를 만들 수 있다.
* History and Achievement
  + 1940년대부터 연구되었지만 2010년 이후에서야 만개함
  + 특히, supervised learning에 큰 성과를 거둠(하지만 다른 분야에도 적용 중)

## Deep Learning 연구의 기폭제: AlexNet의 ImageNet Challenge 우승

* Deep learning이 발전한 이유는 결국 다른 기술들에 유의미한 성과가 있었기 때문임
* Deep learning 발전의 전환점이 된 두 사건
  1. Automatic speech recognition (TIMIT dataset, 2010)
  2. Image recognition (ImageNet challenge, 2012)
* 지속적인 발전의 이유
  1. 컴퓨팅 능력 향상
  2. 이론적 한계를 차근차근 극복해나감
  3. 데이터 셋 확보가 용이해짐
  4. 돈이 됨?, 가능성 있음?
* 이제 간단히 이론에 대해 알아보겠음

## Classficiation에 대한 간략한 소개

* 이론에 앞서 supervised learning의 한 분야인 classification에 대해 알아보자.
* Problem of identifying to which of a set of categories a new observation belongs, on the basis of a training set of data containing observations (or instances) whose category membership is known
* 간단한 설명 - hyperplane
* Neural network가 classification 문제를 잘 풀 수 있는 이유: 고차원, 복잡한 분포의 데이터에서도 적절한 hyperplane을 구성할 '표현력'이 있다. (신의 입장에서는 좋은 classifier를 만들 수 있다)
* 하지만, 여전히 weight들의
* CIFAR-10을 사용하여 three-layer neural network를 만들어서 설명해보도록 하겠다.

## Neural Network의 학습법: Backpropagation

* Loss 함수
* Gradient Descent
* Neural network에 gradient를 계산할 방법이 필요함 -> Backpropagation
* Backpropagation 설명

## Convolutional Neural Network (CNN)

* 필요성: 연산량이 너무 많음
* Motivation: 지역적 정보 활용?
* Architecture 설명


// git add -A; git commit -m "test"; git push
