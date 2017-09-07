---
layout: post
title: TensorFlow 이해하기
categories:
  - TensorFlow
---
자율주행 자동차와 인공지능과 같은 deep learning을 사용하는 서비스들이 두각을 나타내면서 deep learning 플랫폼에 대한 경쟁도 심화되고 있습니다. 하드웨어 플랫폼의 경우 초창기부터 많은 투자를 한 NVIDIA가 많이 앞서 나가고 있습니다. 특히, NVIDIA의 병렬 컴퓨팅 아키텍처인 CUDA의 deep learning 라이브러리 지원(cuBLAS, cuDNN)과 개발자 커뮤니티 규모는 경쟁자들을 압도하고 있습니다[^Dettmers17]. Google의 Tensor Processiong Unit(TPU)를 필두로 여러 회사들이 deep learning을 위한 프로세서를 제안하고 있지만 NVIDIA를 추월하는 것은 쉽지 않을 것으로 보입니다.

![placeholder](https://i.imgur.com/6Ai3QMa.png "Figure 1")
*Figure 1. 2016년 이후 폭발적으로 오른 NVIDIA 주가*

반면 소프트웨어 플랫폼은 상대적으로 경쟁이 치열해 보입니다. Google의 TensorFlow가 플랫폼을 오픈 소스로 공개하고 양질의 튜토리얼과 문서들을 제공하며 선점 효과를 누리고 있지만[^Rubashkin17], 후발 주자들(Microsoft CNTK, MXNet, PyTorch, ...)이 향상된 성능과 편리한 기능들을 선보이고 있기 때문에 안심할 상황은 아닌 것 같습니다[^TensorFlowBlog].

TensorFlow는 천하의 Google이 야심 차게 진행하고 있는 프로젝트이기 때문에 미래가 밝아 보입니다. 하지만, 따라오는 후발 주자들의 기세를 볼 때 이 플랫폼을 state-of-the-art로 단정짓는 것은 성급한 생각인 것 같습니다. 이를 염두에 두고 지금부터 TensorFlow에 대해 알아보도록 하겠습니다.

### Google은 왜 TensorFlow를 오픈 소스로 공개하였을까?

TensorFlow 개발의 책임자는 MapReduce 논문으로 유명한 Jeff Dean입니다[^Jeffrey14]. Jeff Dean은 구글이 TensorFlow를 만들고 오픈 소스로 공개한 이유에 대해 아래와 같이 설명합니다.

> One of the reasons we built TensorFlow, our next-generation system, the system that we’ve actually open sourced for machine learning, is that we wanted to keep the scalable attributes and production readiness of our first system, but make it a much more flexible platform for doing all kinds of machine-learning research and product development [^Jeffrey17]

글에 따르면 TensorFlow를 오픈 소스로 공개하는 주 이유는 MapReduce나 BigTable, Borg와 같은 기술에서 Google이 겪었던 실패를 반복하지 않기 위해서 입니다. 당시 Google은 자체적으로 개발한 기술을 직접 공개하지 않고 whitepaper를 배포하였는데, 외부의 개발자들이 이를 구현한 Hadoop, HBase, Docker 등이 산업 표준이 되어 버리면서 Google이 이에 맞춰야 하는 우스꽝스러운 상황이 반복되었습니다[^Lee16]. Jeff Dean이 주장하는 작전은 Google이 사용하는 기술을 오픈 소스로 공개하여 기술의 장점과 오픈 소스 커뮤니티의 힘을 동시에 취하는 것입니다.

덧붙여, Amazon이 주도권을 쥐고 있는 클라우드 시장의 판도를 뒤집으려는 의도도 보입니다. Deep learning 서비스가 클라우드 시장의 킬러 앱이 되면 Google Cloud Platform과 TensorFlow의 연계는 매우 위력적일 것입니다[^GoogleCloud]. 최근 OpenAI가 Microsoft의 Azure를 deep learning 플랫폼을 선택한 것이 어쩌면 클라우드 시장 지각 변동의 서막일지도 모릅니다[^TensorFlowBlog2].

### TensorFlow 프레임워크의 구조

TensorFlow의 핵심 구성 요소는 개발자가 사용하는 언어 별로 구현되어 있는 client(front-end)와 실제 training과 inference를 수행하는 core execution system(back-end)입니다. 개발자는 client가 제공하는 API를 사용하여 프로그램을 작성할 수도 있고, 이를 기반으로 작성된 training libraries나 inference libraries를 사용할 수도 있습니다. Client와 core execution system 사이에는 이 C API가 존재하는데, client와 core execution system의 작성 언어가 다를 경우 이를 연결해주는 역할을 합니다.

![placeholder](https://i.imgur.com/MUyr3eh.png "Figure 2")
*Figure 2. TensorFlow Architecture [^TensorFlow]*

TensorFlow는 기본적으로 Python, Java, C/C++, Go를 지원하고, 이를 위한 client들이 구현되어 있습니다. 또 추가 언어 지원을 위한 client를 작성할 수도 있습니다. 하지만 문서화나 API의 풍부함을 생각하면, 특별한 이유가 없다면 Python을 선택하는 것이 가장 합리적인 선택입니다[^TensorFlow2]. TensorFlow 뿐만이 아니라 많은 deep learning 소프트웨어 플랫폼들이 Python 지원에 주력하고 있는데, 이는 Python이 machine learning 개발자들이 가장 많이 사용하는 언어이기 때문입니다[^Puget16].

Client의 주요 역할은 TensorFlow 개발자가 작성한 프로그램을 dataflow graph(computational graph)의 형태로 저장하고 수행을 개시하는 것입니다. Dataflow graph의 node는 operation입니다. Operation은 사전에 정의된 작업을 수행하는 연산의 최소 단위입니다. 단순한 덧셈과 뺄셈부터 matrix multiplication, convolution까지 모두 operation으로 정의할 수 있습니다. 각 operation은 0개 이상의 tensor를 입력으로 받아 한 개의 tensor를 출력합니다. Tensor는 TensorFlow에서 데이터를 저장하기 위한 자료구조로, 다차원의 행렬을 저장할 수 있습니다. Operation들이 tensor를 통해 연결되어 있기 때문에 tensor를 dataflow graph의 edge라고 부릅니다. 아래의 figure 3는 TensorFlow 홈페이지에 나와 있는 dataflow graph의 예입니다.

![placeholder](https://i.imgur.com/qvjrgd2.gif "Figure 3")
*Figure 3. Example of Dataflow Graph in TensorFlow [^TensorFlow3]*

Core execution system은 client가 개시한 dataflow graph의 수행을 실제 처리합니다. 위의 figure 2를 다시 보겠습니다. Core execution system 안의 dataflow executor는 dataflow graph를 client로부터 전달 받은 후, CPU나 GPU 같은 device에게 dataflow 상의 각 operation의 kernel을 수행하라는 명령을 내립니다. 이때 kernel은 특정 device에서의 operation의 구현입니다. 예를 들어 TensorFlow에는 matrix multiplication operation에 대해 CPU kernel과 CUDA library를 사용하는 GPU kernel이 따로 구현되어 있습니다. 참고로, core execution system은 처리 성능을 높이기 위해 모두 C++로 구현되어 있습니다.

남은 부분인 distributed master와 networking layer는 TensorFlow의 분산 시스템 지원을 위한 부분입니다. 이 글의 목적은 기본적인 TensorFlow 동작을 이해하는 것이므로 설명하지 않고 넘어가도록 하겠습니다.

### TensorFlow 프로그램의 기본 구조

간단한 예제를 통해 TensorFlow 프로그램 구조를 알아보겠습니다. 예제는 TensorFlow 홈페이지의 튜토리얼 문서에 나오는 MNIST dataset을 이용한 숫자 필기 인식 프로그램입니다[^TensorFlow4]. 아래 figure 4와 같은 가로 세로 28픽셀의 흑백 숫자 이미지 입력으로 받아 0~9 중 어떤 숫자인지를 판별합니다.

![placeholder](https://i.imgur.com/kpjwnOr.png "Figure 4")
*Figure 4. MNIST dataset*

작성할 neural network 구조는 figure 5에 표시하였습니다. Input layer는 각 이미지의 784개의 픽셀을 입력으로 받습니다. Output layer는 input layer의 각 neuron으로부터 픽셀 값을 전달 받아 weighted sum을 계산하여 bias를 더한 후 출력합니다. 이 예제는 매우 단순하기 때문에 어떤 neuron도 activation function을 가지지 않습니다. 최종 출력 값 10개는 각각 0~9의 숫자의 점수를 의미하고, 가장 높은 점수가 높은 숫자를 해당 입력 이미지의 숫자로 결정합니다.

![placeholder](https://i.imgur.com/RkC4KVm.png "Figure 5")
*Figure 5. Very Simple Neural Network*

이 네트워크의 입력과 출력을 각각 벡터 \\(x\\)와 \\(y\\)로 표시하면 둘 사이의 관계식은 아래와 같습니다.

\\[y = Wx + b\\]

이 때, \\(W\\)는 output layer에서 각 neuron들의 weight를 저장하는 \\(10 \times 284\\) 행렬입니다. \\(W\\)의 \\(i\\)번째 행은 output layer의 \\(i\\)번째 neuron의 weight 값들을 저장합니다.

이제 프로그램을 단계별로 설명하겠습니다. 여기서는 프로그램의 구조를 파악하기 위한 최소한의 설명만 할 것입니다. 자세한 설명이 필요할 경우 TensorFlow 홈페이지를 참고하시면 됩니다[^TensorFlow4].

```python
# mnist_softmax.py

from __future__ import absolute_import
from __future__ import division
from __future__ import print_function

import argparse
import sys

from tensorflow.examples.tutorials.mnist import input_data

import tensorflow as tf

FLAGS = None

def main(_):
  # Import data
  mnist = input_data.read_data_sets(FLAGS.data_dir, one_hot=True)

  # Create the model
  x = tf.placeholder(tf.float32, [None, 784])
  W = tf.Variable(tf.zeros([784, 10]))
  b = tf.Variable(tf.zeros([10]))
  y = tf.matmul(x, W) + b

  # Define loss and optimizer
  y_ = tf.placeholder(tf.float32, [None, 10])
  cross_entropy = tf.reduce_mean(
      tf.nn.softmax_cross_entropy_with_logits(labels=y_, logits=y))
  train_step =
      tf.train.GradientDescentOptimizer(0.5).minimize(cross_entropy)

  # Initialize
  sess = tf.InteractiveSession()
  tf.global_variables_initializer().run()

  # Train
  for _ in range(1000):
    batch_xs, batch_ys = mnist.train.next_batch(100)
    sess.run(train_step, feed_dict={x: batch_xs, y_: batch_ys})

  # Test trained model
  correct_prediction = tf.equal(tf.argmax(y, 1), tf.argmax(y_, 1))
  accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))
  print(sess.run(accuracy, feed_dict={x: mnist.test.images,
                                      y_: mnist.test.labels}))

if __name__ == '__main__':
  parser = argparse.ArgumentParser()
  parser.add_argument('--data_dir', type=str,
                      default='/tmp/tensorflow/mnist/input_data',
                      help='Directory for storing input data')
  FLAGS, unparsed = parser.parse_known_args()
  tf.app.run(main=main, argv=[sys.argv[0]] + unparsed)
```

TensorFlow 프로그램은 크게 (1) dataflow graph를 작성하여 원하는 computation을 표현하는 부분과 (2) dataflow graph를 수행하는 부분으로 나눌 수 있습니다. 위의 코드에서 create the model과 define loss and optimizer는 (1)에 해당하고, train과 test trained model은 (2)에 해당합니다.

#### Import data

서버에서 MNIST data set을 가져옵니다.

#### Create the model

\\(y = Wx + b\\)를 계산할 수 있는 dataflow graph를 그립니다. Graph를 작성하는 과정은 매우 직관적입니다. 각 statement는 새로운 tensor를 선언하고 해당 tensor를 생성할 operation을 명시합니다. 즉, 프로그램의 x, W와 b는 tensor이고 placeholder, Variable, matmul와 더하기는 operation입니다. Tensor x를 정의할 때 쓰인 placeholder은 실제 dataflow graph를 수행할 때 입력되는 데이터를 전달하는 operation입니다. 최종적으로 \\(y = Wx + b\\)를 계산한 최종 결과가 tensor y에 저장되는 dataflow graph가 완성되었습니다.

#### Define loss and optimizer

W와 b를 training 시킬 수 있도록 dataflow graph를 확장합니다. y_는 입력으로 받는 각 이미지 데이터 x에 대한 label(정답)을 저장하는 tensor입니다. cross_entropy에는 이 neural network의 loss function을 계산한 결과를 저장합니다. Loss function을 계산하는 과정에서 neural network를 통과한 결과를 저장하는 y와 label이 저장된 y_를 입력으로 받습니다(이에 따라 dataflow graph가 연결됩니다). 참고로 여기서는 softmax regression을 사용하여 loss function을 계산하는데[^CrossEntropy], 이에 대한 설명은 생략하도록 하겠습니다. 마지막으로 train_step이라는 tensor에 W와 b에 대해 gradient descent를 수행할 operation의 결과를 인가합니다. 아래의 figure 6는 완성된 dataflow graph를 TensorBoard라는 도구를 사용하여 출력한 결과입니다.

![placeholder](https://i.imgur.com/J5UvFyv.png "Figure 6")
*Figure 6. Dataflow Graph of MNIST Example*

#### Initialize

실제 dataflow graph를 돌리기 위해서는 session object를 하나 선언합니다. 그리고, 본격적으로 작성한 dataflow graph를 돌리기에 앞서 tensor들을 초기화 시킵니다. 초기화 시킬 변수는 W와 b입니다(앞에서 dataflow graph를 그리는 과정에서 이 두 변수를 0으로 초기화 시킬 것이라고 명시하였습니다).

#### Train

1000번에 걸쳐 training set에서 100개의 batch data를 사용하여 training을 수행합니다. Session의 run() method를 호출할 때 앞서 선언한 train_step이라는 tensor를 입력으로 받습니다. 이러면 train_step을 정의할 때 사용된 GradientDescentOptimizer를 사용하여 training을 실제로 진행합니다. 이때, 앞서 placeholder로 선언된 x와 y_에 들어갈 데이터를 feed_dict을 사용하여 인가해줘야 합니다.

#### Test trained model

Training을 마친 모델에 대해 test set을 사용하여 정확도를 검증합니다. Tensor y와 y_로부터 다시 dataflow graph를 확장하여 정확도를 계산한 결과를 accuracy라는 tensor에 저장하게 하고, 마찬가지로 session의 run method를 사용하여 결과를 출력합니다.

여기까지 TensorFlow 프로그램의 구조에 대해 알아보았습니다. 앞서 이야기한 것처럼 TensorFlow의 프로그램은 (1) dataflow graph를 작성하여 원하는 computation을 표현하는 부분과 (2) dataflow graph를 수행하는 부분으로 나뉩니다. 마지막으로 각 부분이 실제 프레임워크 상에서 어떻게 동작하는지 조금 더 자세히 알아보겠습니다.

### Step 1. Dataflow graph 구축

Dataflow graph를 구축하는 작업은 전적으로 client 단에서 이루어집니다. 앞서 설명한 것처럼 TensorFlow는 지원하는 언어마다 각각 client가 구현되어 있는데, 이 중 가장 많이 쓰이는 Python client를 기준으로 설명하겠습니다.

Python client는 dataflow graph를 관리하기 위해 세 개의 object를 관리합니다. 각 object에 대한 설명은 아래와 같습니다.

* Graph
  + A TensorFlow computation, represented as a dataflow graph
* Operation
  + Represents unit of computation
  + Node of Graph
* Tensor
  + Represents unit of data that flow between operations
  + Edge of Graph

Graph object는 자신이 소유하고 있는 Operation object들을 가리키기 위한 dictionary 자료 구조를 유지합니다. 이를 통한 Python client는 자신이 원하는 Operation object에 쉽게 접근할 수 있습니다. Operation object는 자신의 입력 Tensor object와 출력 Tensor object를 가리키고 있습니다. 마찬가지로, Tensor object는 자신을 출력하는 Operation object와 자신을 입력으로 받는 Operation object를 가리키고 있습니다.

프로그램에 따라 dataflow graph 구축을 마치면 Session의 run() method가 호출됩니다. 그러면 Python client는 C API로 구현된 wrapper function을 호출하여 core execution system이 dataflow graph 수행을 시작할 수 있게 해줍니다. 이 과정에서 Python client가 작성한 그래프 자료구조를 core execution system에게 넘겨주게 됩니다.

### Step 2. Dataflow graph 수행(단일 머신의 경우)

TensorFlow의 core execution system은 두 종류의 Session object를 가지고 있습니다. 하나는 단일 머신에서 사용하는 DirectSession이고 다른 하나는 분산 시스템에서 사용하는 GrpcSession입니다. 여기서는 DirectSession에 대해서면 다루겠습니다.

DirectSession은 전달받은 dataflow graph의 수행을 관리합니다. Dataflow graph를 병렬적으로 수행할 수 있는 선에서 여러 개의 subgraph로 나눕니다. 그 후, subgraph의 개수만큼 Executor라는 object를 생성하고, 각 Executor에 subgraph를 전달하고 수행을 명령합니다. DirectSession는 barrier를 사용하여 모든 Executor가 맡은 일을 마무리할 때까지 대기합니다.

Executor는 subgraph 상의 각 operation의 수행을 Device에게 순차적으로 명령합니다. Device는 시스템 상의 processing unit(CPU나 GPU)마다 하나씩 선언되어 있습니다. Device는 Executor로부터 해당 operation을 수행하기 위한 입력과 kernel을 전달받아 실제 작업을 수행합니다. 특정 operation의 kernel은 해당 operation의 object의 Compute() method에 정의되어 있습니다.

아래의 코드는 matrix multiplication operator의 GPU에 대한 kernel의 예입니다. MatMulOp object의 Compute() method는 LaunchMatMul object의 launch() method를 호출합니다. 이는 TensorFlow의 GPU kernel의 전형적인 프로그래밍 패턴입니다. launch() method는 CUDA에서 지원하는 cuBLAS를 사용하여 matrix multiplication을 수행합니다.

```c++
// tensorflow/core/kernels/matmul_op.cc

template <typename Device, typename T, bool USE_CUBLAS>
class MatMulOp : public OpKernel {
  public:
  void Compute(OpKernelContext* ctx) override {
    // ...
    LaunchMatMul<Device, T, USE_CUBLAS>::launch(
      ctx, this, a, b, dim_pair, out);
  }
};
```

```c++
// tensorflow/core/kernels/matmul_op.cc

template <typename T>
struct LaunchMatMul<GPUDevice, T, true /* USE_CUBLAS */> {
  static void launch(
    OpKernelContext* ctx, OpKernel* kernel, const Tensor& a,
    const Tensor& b,
    const Eigen::array<Eigen::IndexPair<Eigen::DenseIndex>, 1>& dim_pair,
    Tensor* out) {
  // ...
  bool blas_launch_status =
  stream
      ->ThenBlasGemm(blas_transpose_b, blas_transpose_a, n, m, k, 1.0f,
                     b_ptr, transpose_b ? k : n, a_ptr,
                     transpose_a ? m : k, 0.0f, &c_ptr, n)
        .ok();
  // ...
  }
}
```

### 결론

지금까지 TensorFlow의 배경, 철학, 구조와 동작 방식에 대해 간략하게 알아보았습니다. 향후에는 분산 시스템의 경우를 포함한 TensorFlow 내부 동작에 대한 자세한 분석을 다룰 예정입니다.

[^Dettmers17]: <http://timdettmers.com/2017/04/09/which-gpu-for-deep-learning>
[^Rubashkin17]: <https://www.svds.com/getting-started-deep-learning/?utm_campaign=Revue+newsletter&utm_medium=Newsletter&utm_source=revue>
[^TensorFlowBlog]: <https://tensorflow.blog/2017/02/13/chainer-mxnet-cntk-tf-benchmarking>
[^Jeffrey14]: Jeffrey Dean and Sanjay Ghemawat, "MapReduce: simplified data processing on large clusters," USENIX Symposium on Operating Systems Design and Implementation (OSDI), 2014.
[^Jeffrey17]: <https://youtu.be/B0ZnbaOlNss>
[^Lee16]: <https://si.mpli.st/dev/tensorflow-open-source.html>
[^GoogleCloud]: <https://cloud.google.com/ml-engine>
[^TensorFlowBlog2]: <https://tensorflow.blog/2016/11/16/openai-microsoft>
[^TensorFlow]: <https://www.tensorflow.org/extend/architecture>
[^TensorFlow2]: <https://www.tensorflow.org/extend/language_bindings>
[^Puget16]: <https://www.ibm.com/developerworks/community/blogs/jfp/entry/What_Language_Is_Best_For_Machine_Learning_And_Data_Science?lang=en>
[^TensorFlow3]: <https://www.tensorflow.org/programmers_guide/graphs>
[^TensorFlow4]: <https://www.tensorflow.org/get_started/mnist/beginners>
[^CrossEntropy]: <http://colah.github.io/posts/2015-09-Visual-Information/>
