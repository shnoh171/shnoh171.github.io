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

Neuron은 ANN의 가장 작은 연산 단위입니다. 한 neuron은 이전 neuron들로부터 값들을 전달받아 일련의 연산을 처리한 후 다음 neuron에게 결과를 전달합니다. 이 방식은 우리 뇌 속의 neuron의 동작을 매우 단순화시켜 흉내내는 것입니다. 아래의 그림에서 보듯이, 이전 neuron으로 받은 값들은 각각 weight $w_i$를 곱한 후 bias $b$를 더하는 선형 결합으로 합쳐집니다. 그 후 이 값을 바로 다음 neuron에게 전달하는 것이 아니라, activation function이라고 불리는 함수 $f$를 거친 값을 전달합니다. Activation function이 필요한 이유는 nonlinear function을 사용하여 ANN이 표현할 수 있는 함수의 자유도를 높여주기 위해서인데, 이에 대해서는 조금 뒤에 더 설명하도록 하겠습니다.

![placeholder](https://i.imgur.com/5H9IqY4.jpg "Figure 2")
*Figure 2. Neuron [^CS231n17_2]*

Layer는 neuron들의 집합입니다. ANN은 아래의 그림과 같이 여러 개의 layer로 구성되며, 각 layer는 직전 layer의 neuron들로부터 값을 입력으로 전달받아 다음 layer의 neuron들에게 출력값을 전달합니다. Raw data를 직접 입력으로 받는 첫번째 layer를 input layer라고 부르고, 최종 출력을 담당하는 마지막 layer를 output layer라고 부릅니다. 나머지 layer는 hidden layer입니다. 일반적으로 hidden layer가 하나 이상이면 deep learning이라고 부르는데, 어차피 deep learning이라는 단어가 광고성 단어이기 때문에 이런 구분이 크게 중요하지는 않습니다.

![placeholder](https://i.imgur.com/WYV1zUu.jpg "Figure 3")
*Figure 3. Neural Network [^CS231n17_2]*

### 그래서 Artificial Neural Network가 왜 좋나요?

ANN이 왜 좋은지를 설명하기 위해서, supervised learning의 한 갈래인 classification 문제를 예로 들겠습니다. Classification은 주어진 입력 데이터가 주어진 class 중 어떤 class에 속하는지를 판별하는 문제입니다. 사진이 개를 찍은 것인지, 고양이를 찍은 것인지 판단하는 것이 대표적인 classfication 문제입니다. 아래 그림은 아주 단순한 classification의 예를 나타낸 것인데, 두 개의 입력 데이터(사람의 나이, 종양의 크기)를 사용하여 해당 종양이 악성인지 아닌지를 판단하는 문제입니다. 이 문제의 경우, 주어진 training data set을 보고 직선을 하나 그으면 종양의 악성 여부를 효과적으로 판별할 수 있습니다.

![placeholder](https://i.imgur.com/0uJ5UUD.png "Figure 4")
*Figure 4. Simple Classification [^MongoDB]*

하지만 이미지를 보고 이미지에 찍힌 물체의 정체를 파악하는 image recognition의 경우 문제가 복잡해집니다. 우선 입력 데이터가 방대합니다. 가로 세로의 픽셀 수가 512인 저해상도 칼라 이미지만 하더라도 한 이미지의 입력 차원의 수는 \\[512\times512\times3\\]입니다(마지막 3을 곱하는 이유는 RGB 세 색상이 한 픽셀에 들어있기 때문임). 게다가 파악해야 하는 물체의 종류(class)가 많아질수록 \(512\times512\times3\) 차원 공간 속에 데이터의 분포가 매우 복잡해집니다. 이 공간 속에서 위의 간단한 예제에서 선을 그은 것과 같이 hyperplane을 결정해야 하는데, 이를 위해서는 매우 복잡한 함수를 사용하여야 합니다.

기존의 machine learning 기법들은 이런 접근 방식이 불가능했기 때문에, 해당 분야(여기서는 이미지 처리)의 전문가들이 적절한 representation을 추출해냄으로서 문제를 단순화시켰습니다. SIFT feature, HOG feature, SURF feature 등이 이에 해당합니다. 하지만 ANN을 사용할 경우, 굳이 그런 번거로운 작업을 거칠 필요 없이 layer의 수와 layer 안의 neuron 수를 충분히 늘려서 복잡한 hyperplane을 정할 수 있습니다. 물론 정확한 hyperplane을 얻기 위해서는 각 neuron의 weight와 bias들을 잘 조절해야 합니다. 정답이 존재하는 학습 시스템은 구축했지만, 그 정답까지 어떻게 찾아갈 수 있을지는 또 다른 문제인 것이죠. 1940년 대에 제안되었던 ANN은 이에 대한 해답을 내놓지 못하였고 빙하기를 맞게 됩니다. 그 후 1970년대에 ANN의 weight와 bias들을 자동적으로 학습할 수 있는 backpropagation이라는 방법이 제안됩니다.

### Backpropagation 설명에 앞서: Gradient Descent 이해 하기

Backpropagation을 이해하기 위해서는 gradient descent를 이해하고 있어야 합니다. Gradient descent는 machine learning algorithm들이 training 단계에서 사용하는 가장 기본적인 파라미터 최적화 기법입니다. 주어진 training set과 파라미터에 대해 학습이 얼마나 잘 되었는지를 판단하기 위한 loss function(또는 cost function)을 정의하고, 이 함수가 충분히 줄어들 때까지 파라미터를 변경하고 loss function을 계산하는 과정을 반복합니다. Loss function을 어떻게 정할지는 machine learning engineer의 몫인데, 원하는 값과 실제 값 사이에 얼마나 차이가 있는지를 수식으로 표현하여 사용합니다.

![placeholder](https://i.imgur.com/hNpPZWv.png "Figure 5")
*Figure 5. Gradient Descent [^AndrewNg]*

Gradient descent는 안개가 끼어있는 산에서 사람에 내려오는 방법에 비유할 수 있습니다. 지도나 스마트폰이 없다면 우리가 취할 수 있는 유일한 방법은 경사가 낮은 방향으로 한 걸음씩 반복적으로 움직이는 것입니다. 위의 그림은 두 개의 파라미터 $\theta_0$와 $\theta_1$에 대한 loss function $J(\theta_0,\theta_1)$의 값을 표시한 그래프입니다. 그래프 상의 검은 선을 보면 좌측 상단에서 우측 하단으로 loss function의 결과 값이 줄어드는 방향으로(기울기가 가장 가파른 방향으로) 한 걸음씩 내려오는 것을 확인할 수 있습니다.

즉, gradient descent는 주어진 파라미터 $\theta_0$과 $\theta_1$에 대해 gradient of $J$, 즉 $\nabla J(\theta_0,\theta_1)$를 계산한 후 step size $\gamma$만큼씩 각 parameter를 변경하는 것이라고 이야기할 수 있습니다. 이를 수식으로 표현하면 다음과 같습니다.
\\[\theta_0 := \theta_0 - \gamma \frac{\partial}{\partial \theta_0} J(\theta_0,\theta_1)\\]
\\[\theta_1 := \theta_1 - \gamma \frac{\partial}{\partial \theta_1} J(\theta_0,\theta_1)\\]

### Backpropagation: Deep Learning을 위한 Gradient Descent 적용

ANN에 gradient descent를 적용하기 힘든 이유는 weight와 bias값이 너무 많기 때문입니다. Loss function을 $J$라고 하면 각 neuron의 모든 weight $w$와 bias $b$에 대해 $\frac{\partial J}{\partial w}$와 $\frac{\partial J}{\partial b}$를 계산해야 하는데 이것이 보통 일이 아닙니다. 아래의 그림은 2014년 ImageNet Large Scale Visual Recognition Competition(ILSVRC)에서 우승한 GoogleNet의 구조인데 굉장히 복잡하게 layer를 구성했다는 것을 알 수 있습니다. 각 layer에는 다수의 neuron들이 있고, 각 neuron에는 다수의 weight와 bias 파라미터들이 존재합니다.

![placeholder](https://i.imgur.com/K7QjClh.png "Figure 5")
*Figure 6. GoogleNet's Architecture [^Deshpande16]*

Backpropagation은 이런 복잡한 neural network에 대해 gradient를 빠르게 계산할 수 있는 기법입니다.





### Deep Learning 연구의 기폭제: AlexNet의 ImageNet Challenge 우승


### 결론

---

* Neural network에 이를 가능하게 하는 방법이 제안되었으니, 이는 backpropagation이다.
  + TODO: backpropagation 조사하고 정리하여 설명



* Deep learning이 발전한 이유는 결국 다른 기술들에 유의미한 성과가 있었기 때문임
* Deep learning 발전의 전환점이 된 두 사건
  1. Automatic speech recognition (TIMIT dataset, 2010)
  2. Image recognition (ImageNet challenge, 2012)
* 지속적인 발전의 이유
  1. 컴퓨팅 능력 향상
  2. 이론적 한계를 차근차근 극복해나감
  3. 데이터 셋 확보가 용이해짐
  4. 돈이 됨?, 가능성 있음?

### Convolutional Neural Network (CNN)

  * 필요성: 연산량이 너무 많음
  * Motivation: 지역적 정보 활용?
  * Architecture 설명

[^CS231n16_YouTube]: https://www.youtube.com/playlist?list=PLkt2uSq6rBVctENoVBg1TpCC7OQi31AlC
[^LeCun15]: Y. Lecun, Y. Bengio, and G. Hinton, "Deep learning," Nature, 2015.
[^Bengio13]: Y. Bengio, A. Courville, and P. Vincent, "Representation learning: A review and new perspectives," IEEE Transactions on Pattern Analysis and Machine Intelligence, 2013.
[^CS231n17]: http://cs231n.stanford.edu
[^CS231n17_2]: http://cs231n.github.io/neural-networks-1/
[^MongoDB]: https://www.mongodb.com/blog/post/deep-learning-and-the-artificial-intelligence-revolution-part-2
[^AndrewNg]: AndrewNg, "Machine learning," Coursera.
[^Deshpande16]: https://adeshpande3.github.io/adeshpande3.github.io/The-9-Deep-Learning-Papers-You-Need-To-Know-About.html
