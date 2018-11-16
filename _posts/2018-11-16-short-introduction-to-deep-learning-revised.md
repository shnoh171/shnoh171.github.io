---
layout: post
title: A Short Introduction to Deep Learning
categories:
  - Deep Learning
---
This post is written to share the basic knowledge that students and researchers need to know about deep learning. Most of the figures used in the post were taken from Stanford's CS231n course.[^CS231n16_YouTube] I recommand you take this class if you want to continue studying the deep learning.

### What is Deep Learning? 

Yann Lecun, Yoshua Benghio, and Geoffrey Hinton published a paper in Nature that summarizes the development of Deep Learning in 2015.[^LeCun15] In this paper, they define deep learning as follows.

> Deep learning is a class of techniques that allows computational models that are composed of multiple processing layers to learn representations of data with multiple levels of abstraction.

In the above sentence, representation (or feature) is measurable property of data. For example, a machine learning algorithm that looks for spam might use representations such as the number of times a particular word appears, the starting word of the title, and the written language. The performance of a machine learning algorithm is significantly affected by which representations are extracted from given input data. Usually, experts in the field choose which representation to extract based on their prior experiences. However, as the input data grows and problem becomes more complex, it becomes harder for humans to extract the appropriate representation.

/* Currently writing */

Deep learning은 이런 representation 추출 알고리즘을 스스로 학습할 수 있는 기술입니다. 이를 가능하게 하는 이유는 많은 layer를 사용하여 데이터를 여러 단계에 걸쳐 추상화시키기 때문입니다. 아래의 그림은 Stanford 대학의 CS231n 강좌에서 가져온 간단한 deep learning의 동작 그림입니다. 왼쪽에 사람이 말을 타고 있는 사진이 입력 데이터로 들어가고, 이 데이터가 오른쪽으로 layer를 거칠 때마다 점점 추상화된 형태로 가공됩니다. 마지막 layer를 지나고 오른쪽 끝에 도착했을 때, 말을 나타내는 8번째 representation이 가장 높은 값(흰색)을 가지게 되고, 이에 따라 이 사진을 말이라고 판별하게 됩니다.

![placeholder](https://i.imgur.com/ahRk6zc.png "Figure 1")
*Figure 1. Image Recognition with Deep Learning [^CS231n17]*

이렇게 여러 개의 layer로 구성된 학습 시스템을 일반적으로 artificial neural network(ANN)라고 부릅니다. Neural network라고 부르는 이유는 인간의 뇌의 동작에서 힌트를 얻어 만들어졌기 때문입니다. Deep learning은 한마디로 ANN을 사용하는 machine learning 기술의 집합입니다. 사실 ANN의 개념은 1940년대에 제안되었지만 오랜 기간 동안 주목 받지 못했었고, 2000년대 들어 이 분야가 다시 주목받기 시작하면서 관련 연구자들이 "Deep Learning"이라는 새로운 단어를 유행시켰습니다. 그 후 deep learning은 machine learning의 한 분류인 supervised learning에서 큰 성과를 거뒀고, 현재는 unsupervised learning과 reinforcement learning에서도 활발히 연구되고 있습니다.

### Artificial Neural Network의 기본 구조

Neuron은 ANN의 가장 작은 연산 단위입니다. 한 neuron은 이전 neuron들로부터 값들을 전달받아 일련의 연산을 처리한 후 다음 neuron에게 결과를 전달합니다. 이 방식은 우리 뇌 속의 neuron의 동작을 매우 단순화시켜 흉내 내는 것입니다. 아래의 그림에서 보듯이, 이전 neuron으로 받은 값들은 각각 weight \\(w_i\\)를 곱한 후 bias \\(b\\)를 더하는 선형 결합으로 합쳐집니다. 이 값을 바로 다음 neuron에게 전달하는 것이 아니라, activation function이라고 불리는 함수 \\(f\\)를 거친 값을 전달합니다. Activation function이 필요한 이유는 nonlinear function을 사용하여 ANN이 표현할 수 있는 함수의 자유도를 높여주기 위해서인데, 이에 대해서는 조금 뒤에 더 설명하도록 하겠습니다.

![placeholder](https://i.imgur.com/5H9IqY4.jpg "Figure 2")
*Figure 2. Neuron [^CS231n17_2]*

Layer는 neuron들의 집합입니다. ANN은 아래의 그림과 같이 여러 개의 layer로 구성되며, 각 layer의 neuron은 직전 layer의 neuron들로부터 입력을 전달받아 계산을 수행하고 다음 layer의 neuron들에게 출력을 전달합니다. Raw data를 직접 입력으로 받는 첫번째 layer를 input layer라고 부르고, 최종 출력을 담당하는 마지막 layer를 output layer라고 부릅니다. 나머지 layer는 hidden layer입니다. 일반적으로 hidden layer가 하나 이상이면 deep learning이라고 부르는데, 어차피 deep learning이라는 단어가 광고성 단어이기 때문에 이런 구분이 크게 중요하지는 않습니다.

![placeholder](https://i.imgur.com/WYV1zUu.jpg "Figure 3")
*Figure 3. Neural Network [^CS231n17_2]*

### 그래서 Artificial Neural Network가 왜 좋나요?

ANN이 왜 좋은지를 설명하기 위해서, supervised learning의 한 갈래인 classification 문제를 예로 들겠습니다. Classification은 주어진 입력 데이터가 주어진 class 중 어디에 속하는지를 판별하는 문제입니다. 사진이 개를 찍은 것인지, 고양이를 찍은 것인지 판단하는 것이 대표적인 classification 문제입니다. 아래 그림은 아주 단순한 classification의 예를 나타낸 것인데, 두 개의 입력 데이터(사람의 나이, 종양의 크기)를 사용하여 해당 종양이 악성인지 아닌지를 판단하는 문제입니다. 이 문제의 경우, 주어진 training data set을 보고 직선을 하나 그으면 종양의 악성 여부를 효과적으로 판별할 수 있습니다.

![placeholder](https://i.imgur.com/0uJ5UUD.png "Figure 4")
*Figure 4. Simple Classification [^MongoDB]*

하지만 이미지를 보고 이미지에 찍힌 물체의 정체를 파악하는 image recognition의 경우 문제가 복잡해집니다. 우선 입력 데이터가 방대합니다. 가로 세로의 픽셀 수가 512인 저해상도 칼라 이미지만 하더라도 한 이미지의 입력 차원의 수는 \\(512\times512\times3\\)입니다(마지막 3을 곱하는 이유는 RGB 세 색상이 한 픽셀에 들어있기 때문입니다). 게다가 파악해야 하는 물체의 종류(class)가 많아질수록 \\(512\times512\times3\\) 차원 공간 속에 데이터의 분포가 매우 복잡해집니다. 이 공간 속에서 위의 간단한 예제에서 선을 그은 것과 같이 hyperplane을 결정해야 하는데, 이를 위해서는 매우 복잡한 함수를 사용하여야 합니다.

기존의 machine learning 기법들은 이런 접근 방식이 불가능했기 때문에, 해당 분야(여기서는 이미지 처리)의 전문가들이 적절한 representation을 추출하여 문제를 단순화시켰습니다. SIFT feature, HOG feature, SURF feature 등이 대표적인 예입니다. 하지만 ANN을 사용할 경우, 굳이 그런 번거로운 작업을 거칠 필요 없이 layer의 수와 layer 안의 neuron 수를 충분히 늘려서 복잡한 hyperplane을 정할 수 있습니다. 물론 정확한 hyperplane을 얻기 위해서는 각 neuron의 weight와 bias들을 잘 조절해야 합니다. 정답이 존재하는 학습 시스템은 구축했지만, 그 정답까지 어떻게 찾아갈 수 있을지는 또 다른 문제인 것이죠. 1940년 대에 제안되었던 ANN은 이에 대한 해답을 내놓지 못하였고 암흑기를 맞게 됩니다(AI 쪽 사람들은 이를 흔히 AI winter라고 부릅니다). 그 후 1970년대에 ANN의 weight와 bias들을 자동적으로 학습할 수 있는 backpropagation이라는 방법이 제안됩니다.

### Backpropagation 설명에 앞서 Gradient Descent 이해하기

Backpropagation을 이해하기 위해서는 gradient descent를 이해하고 있어야 합니다. Gradient descent는 machine learning algorithm들이 training 단계에서 사용하는 가장 기본적인 파라미터 최적화 기법입니다. 주어진 training set과 파라미터에 대해 학습이 얼마나 잘 되었는지를 판단하기 위한 loss function(또는 cost function)을 정의하고, 이 함수가 충분히 줄어들 때까지 파라미터를 변경하고 loss function을 계산하는 과정을 반복합니다. Loss function을 어떻게 정할 지는 machine learning engineer의 몫인데, 원하는 값과 실제 값 사이에 얼마나 차이가 있는지를 수식으로 표현하여 사용합니다.

![placeholder](https://i.imgur.com/hNpPZWv.png "Figure 5")
*Figure 5. Gradient Descent [^AndrewNg]*

Gradient descent는 안개가 끼어있는 산에서 사람에 내려오는 방법에 비유할 수 있습니다. 지도나 스마트폰이 없다면 우리가 취할 수 있는 유일한 방법은 경사가 낮은 방향으로 한 걸음씩 반복적으로 움직이는 것입니다. 위의 그림은 두 개의 파라미터 \\(\theta_0\\)와 \\(\theta_1\\)에 대한 loss function \\(J(\theta_0,\theta_1)\\)의 값을 표시한 그래프입니다. 그래프 상의 검은 선을 보면 좌측 상단에서 우측 하단으로 loss function의 결과 값이 줄어드는 방향으로(기울기가 가장 가파른 방향으로) 한 걸음씩 내려오는 것을 확인할 수 있습니다.

즉, gradient descent는 주어진 파라미터 \\(\theta_0\\)과 \\(\theta_1\\)에 대해 gradient of \\(J\\), 즉 \\(\nabla J(\theta_0,\theta_1)\\)를 계산한 후 step size \\(\gamma\\)만큼씩 각 parameter를 변경하는 작업을 반복하는 것입니다. 이 작업을 수식으로 표현하면 다음과 같습니다.
\\[\theta_0 := \theta_0 - \gamma \frac{\partial}{\partial \theta_0} J(\theta_0,\theta_1)\\]
\\[\theta_1 := \theta_1 - \gamma \frac{\partial}{\partial \theta_1} J(\theta_0,\theta_1)\\]

### Backpropagation: Deep Learning을 위한 Gradient Descent 적용

ANN에 gradient descent를 적용하기 힘든 이유는 weight와 bias값이 너무 많기 때문입니다. Loss function을 \\(J\\)라고 하면 각 neuron의 모든 weight \\(w\\)와 bias \\(b\\)에 대해 \\(\frac{\partial J}{\partial w}\\)와 \\(\frac{\partial J}{\partial b}\\)를 계산해야 하는데 이것이 보통 일이 아닙니다. 아래 그림에 표시한 2014년 ILSVRC에서 우승한 GoogleNet만 봐도 굉장히 복잡하게 layer를 구성했다는 것을 알 수 있습니다. 각 layer마다 존재하는 다수의 neuron들의 파라미터들에 대해 모두 gradient를 계산할 수 있어야 gradient descent를 적용할 수 있습니다.

![placeholder](https://i.imgur.com/K7QjClh.png "Figure 6")
*Figure 6. GoogleNet's Architecture [^Deshpande16]*

Backpropagation은 이런 복잡한 neural network에 대해 gradient를 빠르게 계산할 수 있는 기법입니다. 아래의 그림은 CS231n 강좌에 나와 있는 간단한 backpropagation 예제입니다. \\(w_0\\), \\(x_0\\), \\(w_1\\), \\(x_1\\), \\(w_2\\)의 초기값을 정하면 네트워크의 왼쪽으로부터 오른쪽으로 연산이 진행되어 최종 결과값인 0.73이 출력됩니다. 이때 각 neuron의 중간 계산값을 위에 초록색 숫자로 표시하였습니다. Backpropagation은 제일 오른쪽 neuron의 출력에서 시작하여, 역으로 왼쪽으로 진행하며, 각 중간 결과값의 최종 출력값에 대한 변화율(gradient)를 계산합니다. 네트워크를 거꾸로 흐르며 각 중간 결과값의 gradient를 계산하기 때문에 이 기법을 backpropagation이라고 부릅니다.

![placeholder](https://i.imgur.com/2UZZH3C.png "Figure 7")
*Figure 7. Backpropagation Example [^CS231n17_3]*

아래의 그림은 하나의 neuron에 대해 backpropagation이 어떻게 진행되는지를 좀 더 자세히 설명하는 그림입니다. Backpropagation algorithm은 출력 \\(y\\)의 최종값 \\(L\\)에 대한 gradient \\(\frac{\partial L}{\partial y}\\)를 알고 있다는 가정 하에 입력 \\(x\\)의 최종값 \\(L\\)에 대한 gradient \\(\frac{\partial L}{\partial x}\\)를 빠르게 계산해줍니다. 이는 \\(y\\)에 대한 \\(x\\)의 gradient를 구해서 \\(\frac{\partial L}{\partial y}\\)에 곱한 값입니다. 기본적인 미적분학의 chain rule을 이해하고 있으면 쉽게 이 방법을 증명할 수 있습니다.

![placeholder](https://i.imgur.com/RXDNy7J.png "Figure 8")
*Figure 8. Backpropagation of a Neuron*

### 결론

지금까지 기본적인 neural network의 구조와 이 네트워크를 학습시킬 수 있는 기본적인 기법인 backpropagation에 대해 알아보았습니다. 하지만 1970년대에 backpropagation이 제안된 이후에도 deep learning의 침체기는 끝나지 않았습니다. 그 이유는 아래와 같습니다.

1. 학습에 필요한 연산량이 여전히 너무 많았습니다. 이는 이후 연산량을 줄일 수 있는 여러 기법들의 제안과 컴퓨터 하드웨어의 발전에 따라 해결됩니다.
2. 데이터의 양이 적었습니다. 이 역시 인터넷과 전자기기들이 발전되며 해결됩니다.
3. 마지막으로 여러 이론적 한계 때문에 학습이 여전히 잘 이루어지지 않았습니다. 대표적인 예는 vanishing gradient라는 문제였는데, backpropagation을 진행함에 따라 전달되는 gradient 값이 거의 0이 되어버려 파라미터 갱신이 되지 않는 문제였습니다. 이런 문제들 역시 deep learning 계의 학자들에 의해 차근차근 극복되었습니다.

Deep learning이 성과를 보인 대표적인 사건은 ILSVRC 2012에서의 AlexNet의 우승입니다[^Krizhevsky12]. Geoffrey Hinton 교수의 제자였던 Alex Krizhevsky는 convolutional neural network(CNN)라는 특수한 neural network 구조를 사용하여 image recognition 대회에서 오차율 15.4%를 기록하며 오차율 26.2%인 2등을 찍어 누르며 우승합니다. 그 이후 ILSVRC 대회의 우승자들은 모두 CNN을 사용한 모델을 사용하고 있습니다.

지금까지 가장 기본적인 deep learning 이론에 대해 알아보았습니다. 다음 포스트에서는 CNN에 대한 설명과 함께 image recognition 분야에서 deep learning의 발전에 어떻게 이루어졌는지에 대한 글을 적어보려 합니다.

[^CS231n16_YouTube]: <https://www.youtube.com/playlist?list=PLkt2uSq6rBVctENoVBg1TpCC7OQi31AlC>
[^LeCun15]: Y. Lecun, Y. Bengio, and G. Hinton, "Deep learning," Nature, 2015.
[^Bengio13]: Y. Bengio, A. Courville, and P. Vincent, "Representation learning: A review and new perspectives," IEEE Transactions on Pattern Analysis and Machine Intelligence, 2013.
[^CS231n17]: <http://cs231n.stanford.edu>
[^CS231n17_2]: <http://cs231n.github.io/neural-networks-1>
[^MongoDB]: <https://www.mongodb.com/blog/post/deep-learning-and-the-artificial-intelligence-revolution-part-2>
[^AndrewNg]: Andrew Ng, "Machine learning," Coursera.
[^Deshpande16]: <https://adeshpande3.github.io/adeshpande3.github.io/The-9-Deep-Learning-Papers-You-Need-To-Know-About.html>
[^CS231n17_3]: <http://cs231n.github.io/optimization-2>
[^LeCun98]: Y. Lecun, L. Bottou, Y. Bengio, and P. Haffner, "Gradient-based learning applied to document recognition,"  Proceedings of the IEEE, 1998.
[^Krizhevsky12]: A. Krizhevsky, I. Sutskever, and G. E. Hinton, "ImageNet Classification with Deep Convolutional Neural Networks," Advances in Neural Information Processing Systems(NIPS), 2012.
