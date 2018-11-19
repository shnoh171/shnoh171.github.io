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

In the above sentence, representation (or feature) is measurable property of data. For example, a machine learning algorithm that looks for spam might use representations such as the number of times a particular word appears, the starting word of the title, and the written language. The performance of a machine learning algorithm is significantly affected by which representations are extracted from given input data.[^Bengio13] Usually, experts in the field choose representations based on their prior experiences. However, as the size of data grows and problem becomes more complex, it becomes harder for humans to extract the appropriate representations.

/* Currently writing */

### Goal of Neural Network

### Basic Structure of the Artificial Neural Network

### So Why Do People Prefer the Neural Network in Many Cases?

### Understanding the Gradient Descent Before Backpropagation

### Backpropagation: Application of Gradient Descent for Deep Learning

### Conclusions




![placeholder](https://i.imgur.com/ahRk6zc.png "Figure 1")
*Figure 1. Image Recognition with Deep Learning [^CS231n17]*

![placeholder](https://i.imgur.com/5H9IqY4.jpg "Figure 2")
*Figure 2. Neuron [^CS231n17_2]*

![placeholder](https://i.imgur.com/WYV1zUu.jpg "Figure 3")
*Figure 3. Neural Network [^CS231n17_2]*

![placeholder](https://i.imgur.com/0uJ5UUD.png "Figure 4")
*Figure 4. Simple Classification [^MongoDB]*


![placeholder](https://i.imgur.com/hNpPZWv.png "Figure 5")
*Figure 5. Gradient Descent [^AndrewNg]*

\\[\theta_0 := \theta_0 - \gamma \frac{\partial}{\partial \theta_0} J(\theta_0,\theta_1)\\]
\\[\theta_1 := \theta_1 - \gamma \frac{\partial}{\partial \theta_1} J(\theta_0,\theta_1)\\]


![placeholder](https://i.imgur.com/K7QjClh.png "Figure 6")
*Figure 6. GoogleNet's Architecture [^Deshpande16]*

![placeholder](https://i.imgur.com/2UZZH3C.png "Figure 7")
*Figure 7. Backpropagation Example [^CS231n17_3]*

![placeholder](https://i.imgur.com/RXDNy7J.png "Figure 8")
*Figure 8. Backpropagation of a Neuron*

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
