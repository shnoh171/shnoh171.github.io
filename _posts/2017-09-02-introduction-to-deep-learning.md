---
layout: post
title: Deep Learning에 대한 간단한 소개
categories:
  - Basic Theory
---
**아직 미완성인 포스트입니다.** 본 포스트는 deep learning을 처음 접하는 학생들과 연구원들이 알아야 하는 배경과 기초 지식들을 공유하기 위해 작성되었습니다.

### Deep Learning이란 무엇인가
2015년, deep learning을 이야기할 때 절대 빠질 수 없는 대가 세 사람(Yann Lecun, Yoshua Bengio, Geoffrey Hinton)이 Nature에 "Deep Learning"이라는 제목의 논문을 게재하였습니다[^LeCun15]. 이들은 논문에서 정의하는 deep learning이라는 개념을 정리하면 아래의 한 문장으로 요약할 수 있습니다.

>Deep learning is a class of techniques that allows computational models that are composed of multiple processing layers to learn representations of data with multiple levels of abstraction .

1. Class of techniques
    + Deep learning은 machine learning에 사용되는 기술 중 하나입니다.
2. Representations of data
  + sdf
3. Multiple processing layers:


// class of techniques, computational models, multiple processing layers, learn representations of data, multiple levels of abstrction

---
* Deep learning은 neural network를 사용하는 machine learning 기술의 집합이다.
  + Deep learning is a class of techniques which allows computational models that are composed of multiple processing layers to learn representations of data with multiple levels of abstraction
  + 결국은 neural network를 꾸미는 advertise term
  + 특히 supervised learning에 큰 성과를 거둠(하지만 다른 분야에도 적용 중)
  + TODO: 누가 deep learning이라는 단어를 유행시켰는지 조사
* Neural network는 neuron들을 모아 layer를 구성한 network이다.
  + Neuron은 사람의 neuron의 동작을 매우 단순화시킨 것
  + Neuron에 대한 수식적 설명
  + Neuron이 모여 layer를 구성하고 layer를 모아 neural network를 구성
  + 무엇이 좋은가? - 복잡한 함수를 만들 수 있다.
  + 예: Classification을 이용한 복잡한 함수를 만들 수 있다는 증거
    - Problem of identifying to which of a set of categories a new observation belongs, on the basis of a training set of data containing observations (or instances) whose category membership is known
    - 아무리 차원이 높아지고, 분포가 복잡해줘도 이를 분류할 수 있는 hyperplane을 '표현'할 수 있다. (신의 입장에서는 가능함)
    - TODO: 설명 그림 자료 추가

### 어떻게 학습할 것인가?

* 학습의 필요성
  + 이제 당연히 해결되어야 하는 문제는 '학습'을 어떻게 하느냐이다. Weight와 bias 값들을 '잘' 잡는 방법이 있어야 한다.
  + Deep learning의 기본적인 아이디어는 50년대에 나왔지만, 학습을 효율적으로 하는 방법을 찾지 못해 빙하기가 찾아왔다.
* Gradient descent에 대한 설명
  + NN의 학습 방법에 대해 이해하기에 앞서, machine learning의 가장 기본적인 학습법인 gradient descent에 대해 이해하여야 한다.
  + Loss function에 대한 설명
  + Gradient descent는 산에서 내려가는 방법에 비유할 수 있음
  + 2개의 parameter를 가지는 경우의 gradient descent
* Neural network에 이를 가능하게 하는 방법이 제안되었으니, 이는 backpropagation이다.
  + TODO: backpropagation 조사하고 정리하여 설명

### Deep Learning 연구의 기폭제: AlexNet의 ImageNet Challenge 우승

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

[^LeCun15]: Yann Lecun, Yoshua Bengio, and Geoffrey Hinton, "Deep learning," Nature, 2015.



### Convolutional Neural Network (CNN)

* 필요성: 연산량이 너무 많음
* Motivation: 지역적 정보 활용?
* Architecture 설명


// git add -A; git commit -m "test"; git push
