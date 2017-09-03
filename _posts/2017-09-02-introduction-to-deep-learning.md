---
layout: post
title: Deep Learning에 대한 간단한 소개
categories:
  - Basic Theory
---
**아직 미완성인 포스트입니다.** 본 포스트는 deep learning을 처음 접하는 학생들과 연구원들이 알아야 하는 배경과 기초 지식들을 공유하기 위해 작성되었습니다. 이 글에서 사용하는 그림들은 대부분 Stanford의 CS231n 강좌에서 가져왔습니다. 글을 읽고 deep learning에 대해 계속 공부하시려면 이 강의를 들어 보시는 것을 추천합니다[^CS231n16_YouTube].

### Deep Learning이란 무엇인가
2015년, deep learning의 대가 세 사람(Yann Lecun, Yoshua Bengio, Geoffrey Hinton)이 Nature에 deep learning의 발전을 정리하는 논문을 게재하였습니다[^LeCun15]. 이 논문에서 정의하는 deep learning은 아래의 한 문장으로 요약할 수 있습니다.

> Deep learning is a class of techniques that allows computational models that are composed of multiple processing layers to learn representations of data with multiple levels of abstraction .

위의 문장에서 이야기하는 representation(또는 feature)은 데이터의 측정 가능한 특성을 의미합니다. 예를 들어 스팸 메일을 찾는 machine learning 알고리즘에서는 한 이메일에서 특정 단어의 반복 횟수, 작성 언어, 문법의 정확도, 제목의 시작 단어 등이 feature가 될 수 있습니다. 머신 러닝 알고리즘의 성능은 일반적으로 주어진 입력 데이터에서 어떤 representation을 추출하는지에 크게 영향을 받습니다[^Bengio13]. 일반적으로 해당 분야의 전문가들이 자신들의 경험에 기반하여 representation 추출 알고리즘을 개발하여 사용합니다. 하지만 입력 데이터의 차원이 커지고 풀어야 하는 문제가 복잡해질수록 인간이 적절한 representation을 추출하는 알고리즘을 만들기 힘들어집니다.

Deep learning은 이런 representation 추출 알고리즘을 스스로 학습할 수 있는 기술입니다. 이를 가능하게 하는 이유는 많은 layer를 사용하여 여러 단계에 걸친 데이터 추상화 작업을 진행하기 때문입니다. 아래의 그림은 Stanford 대학의 CS231n 강좌에서 가져온 간단한 deep learning의 동작 그림입니다. 왼쪽에 사람이 말을 타고 있는 사진이 입력 데이터로 들어가고, 이 데이터가 오른쪽으로 layer를 거칠 때마다 점점 추상화된 형태로 가공됩니다. 마지막 layer를 지나고 오른쪽 끝에 도착했을 때, 말을 나타내는 8번째 representation이 가장 높은 값(흰색)을 가지게 되고, 이에 따라 이 사진을 말이라고 판별하게 됩니다.

![placeholder](https://i.imgur.com/ahRk6zc.png "Figure 1")
*Figure 1. Image Recognition with Deep Learning [^CS231n17]*

이렇게 여러 개의 layer로 구성된 학습 시스템을 일반적으로 artificial neural network(ANN)라고 부릅니다. Neural network라고 부르는 이유는 인간의 뇌의 동작에서 힌트를 얻어 만들어졌기 때문입니다. Deep learning은 한마디로 ANN을 사용하는 machine learning 기술의 집합입니다. 사실 ANN의 개념은 1940년대에 제안되었지만 오랜 기간 동안 주목 받지 못했었고, 2000년대 들어 이 분야가 다시 주목받기 시작하면서 관련 연구자들이 "Deep Learning"이라는 새로운 단어를 유행시켰습니다. 그 후 deep learing은 machine learning의 한 분류인 supervised learning에서 큰 성과를 거뒀고, 현재는 unsupervised learning과 reinforcement learning에서도 활발히 연구되고 있습니다.

### Artificial Neural Network의 기본 구조

Neuron은 ANN의 가장 작은 연산 단위입니다.

---
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

[^CS231n16_YouTube]: https://www.youtube.com/playlist?list=PLkt2uSq6rBVctENoVBg1TpCC7OQi31AlC
[^LeCun15]: Y. Lecun, Y. Bengio, and G. Hinton, "Deep learning," Nature, 2015.
[^Bengio13]: Y. Bengio, A. Courville, and P. Vincent, "Representation Learning: A Review and New Perspectives," IEEE Transactions on Pattern Analysis and Machine Intelligence, 2013.
[^CS231n17]: http://cs231n.stanford.edu


### Convolutional Neural Network (CNN)

* 필요성: 연산량이 너무 많음
* Motivation: 지역적 정보 활용?
* Architecture 설명


// git add -A; git commit -m "test"; git push
