---
layout: post
title: TensorFlow 이해하기
categories:
  - TensorFlow
---
**아직 작성 중인 글입니다.**

자율주행 자동차와 인공지능과 같은 deep learning을 사용하는 서비스들이 두각을 나타내면서 deep learning 플랫폼에 대한 경쟁도 심화되고 있습니다. 하드웨어 플랫폼의 경우 초창기부터 많은 투자를 한 NVIDIA가 많이 앞서 나가고 있습니다. 특히, NVIDIA의 병렬 컴퓨팅 아키텍처인 CUDA의 deep learning 라이브러리 지원(cuBLAS, cuDNN)과 개발자 커뮤니티 규모는 경쟁자들을 압도하고 있습니다[^Dettmers17]. Google의 Tensor Processiong Unit(TPU)를 필두로 여러 회사들이 deep learning을 위한 프로세서를 제안하고 있지만 NVIDIA를 추월하는 것은 쉽지 않을 것으로 보입니다.

![placeholder](https://i.imgur.com/6Ai3QMa.png "Figure 1")
*Figure 1. 2016년 이후 폭발적으로 오른 NVIDIA 주가*

반면 소프트웨어 플랫폼은 상대적으로 경쟁이 치열해 보입니다. Google의 TensorFlow가 빠르게 플랫폼을 오픈 소스로 공개하고 양질의 튜토리얼과 문서들을 제공하며 선점 효과를 누리고 있지만[^Rubashkin17], 후발 주자들(Microsoft CNTK, MXNet, PyTorch, ...)이 향상된 성능과 편리한 기능들을 선보이고 있기 때문에 안심할 상황은 아닙니다[^TensorFlowBlog].

TensorFlow는 천하의 Google이 야심차게 진행하고 있는 프로젝트이기 때문에 미래가 밝겠지만, 위와 같은 이유로 이 플랫폼을 state-of-the-art로 단정짓는 것은 성급한 생각인 것 같습니다. 이를 염두에 두고 지금부터 TensorFlow에 대해 알아보도록 하겠습니다.

### Google은 왜 TensorFlow를 오픈 소스로 공개하였을까?

TensorFlow 개발의 책임자는 MapReduce 논문으로 유명한 Jeff Dean입니다[^Jeffrey14]. Jeff Dean은 구글이 TensorFlow를 만들고 오픈 소스로 공개한 이유에 대해 아래와 같이 설명합니다.

> One of the reasons we built TensorFlow, our next-generation system, the system that we’ve actually open sourced for machine learning, is that we wanted to keep the scalable attributes and production readiness of our first system, but make it a much more flexible platform for doing all kinds of machine-learning research and product development [^Jeffrey17]

오픈 소스로 공개하는 주 이유는 MapReduce나 BigTable, Borg와 같은 기술에서 겪었던 실수를 반복하지 않기 위해서 입니다. 당시 Google은 자체적으로 개발한 기술을 직접 공개하지 않고 whitepaper를 만들었는데, 외부의 개발자들이 이를 구현한 Hadoop, HBase, Docker 등이 산업 표준이 되어 버리면서 Google이 이에 맞춰야 하는 우스꽝스러운 상황이 반복되었습니다[^Lee16]. Jeff Dean이 주장하는 작전은 Google이 사용하는 기술을 오픈 소스화하여 기술의 장점과 오픈 소스 커뮤니티의 힘을 동시에 취하는 것입니다.

덧붙여, Amazon이 주도권을 쥐고 있는 클라우드 시장의 판도를 뒤집으려는 의도도 보입니다. Deep learning 클라우드 시장의 킬러 앱이 될 경우, 현재 진행 중인 Google Cloud Platform과 TensorFlow의 연계는 위력을 발휘할 수 밖에 없습니다[^GoogleCloud]. 최근 OpenAI가 Microsoft의 Azure를 deep learning 플랫폼을 선택한 것도 어쩌면 이런 클라우드 시장의 지각 변동의 징조일지도 모릅니다[^TensorFlowBlog2].

### TensorFlow 프레임워크의 구조

TensorFlow의 핵심 구성 요소는 개발자가 사용하는 언어 별로 구현되어 있는 client(front-end)와 실제 training과 inference를 수행하는 core execution system(back-end)입니다. 개발자는 client가 제공하는 API를 사용하여 프로그램을 작성할 수도 있고, 이를 기반으로 작성된 training libraries나 inference libraries를 사용할 수도 있습니다. Client와 core execution system 사이에는 이 C API가 존재하는데, client와 core execution system의 작성 언어가 다를 경우 이를 연결해주는 역할을 합니다.

![placeholder](https://i.imgur.com/MUyr3eh.png "Figure 2")
*Figure 2. TensorFlow Architecture [^TensorFlow]*

TensorFlow는 기본적으로 Python, Java, C/C++, Go 언어를 지원하고, 이를 위한 client가 구현되어 있습니다. 추가 언어 지원을 위한 client를 작성할 수도 있습니다. 하지만, 문서화나 API의 제공 정도를 고려할 때, 특별한 이유가 없다면 Python을 선택하는 것이 가장 합리적인 선택입니다[^TensorFlow2]. TensorFlow 뿐만이 아니라 많은 deep learning 소프트웨어 플랫폼들이 Python 지원에 주력하고 있는데, 이는 machine learning 개발자들이 가장 많이 쓰는 언어이기 때문입니다[^Puget16].

Client의 주요 역할은 TensorFlow 개발자가 작성한 프로그램을 dataflow graph의 형태로 저장하고 수행을 개시하는 것입니다. Dataflow graph의 node는 operation입니다. Operation은 사전에 정의된 작업을 수행하는 연산의 최소 단위입니다. 단순한 덧셈과 뺄셈부터 matrix multiplication, convolution까지 모두 operation이 될 수 있습니다. Operation의 입출력으로 사용되는 자료구조는 tensor입니다. Tensor는 다차원의 행렬을 표현하기 위하여 TensorFlow에서 사용하는 자료구조입니다. 아래의 그림은 TensorFlow 홈페이지에 나와 있는 dataflow graph의 예입니다.

![placeholder](https://i.imgur.com/qvjrgd2.gif "Figure 3")
*Figure 3. Example of Dataflow Graph in TensorFlow [^TensorFlow3]*

Client가 개시한 dataflow graph의 수행을 실제 처리하는 부분은 core execution system입니다. Figure 2 상에서 dataflow executor는 dataflow graph를 client로부터 전달 받은 후, device에게 dataflow 상의 각 operation의 kernel을 수행하라는 명령을 내립니다. Device는 CPU나 GPU와 같은 operation을 수행할 수 있는 processing unit입니다. Kernel은 한 operation의 특정 device에 대한 구현을 의미합니다. 시스템 상에 여러 device가 존재할 경우 여러 개의 kernel을 구현해서 사용할 수 있습니다. 예를 들어, TensorFlow에는 matrix multiplication operation에 대해 CPU kernel과 CUDA library를 사용하는 GPU kernel이 따로 구현되어 있습니다.

Distributed master와 networking layer는 TensorFlow의 분산 시스템에서의 동작을 지원하기 위한 부분입니다. 이 글의 목적은 기본적인 TensorFlow 동작을 이해하는 것이므로 분산 시스템의 경우는 고려하지 않도록 하겠습니다.

### TensorFlow 프로그램의 기본 구조

![placeholder](https://i.imgur.com/kpjwnOr.png "Figure 4")
*Figure 3. MNIST dataset*

아래의 프로그램은 deep learning 계의 hello world라고 이야기할 수 있는 MNIST dataset을 이용한 숫자 필기 인식 프로그램입니다[^TensorFlow4]. 각 입력 이미지는 위의 그림과 같은 가로 세로 28픽셀의 흑백 이미지입니다. Input layer는 이미지의 각 픽셀을 입력으로 받고 output layer는 10개의 숫자 각각에 대한 점수를 매깁니다. 파라미터 최적화를 위한 loss function 계산에는 softmax regression을 사용하였습니다. 코드를 읽어보고, 각 부분에 대해 간단히 설명하겠습니다.

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
#### Import data

#### Create the model

#### Define loss and optimizer

#### Train

#### Test trained model



// Expressing computation via Graph과 Executing computation

### Step 1. Dataflow graph를 사용한 computation 표현
* tf.Graph
  + A TensorFlow computation, represented as a dataflow graph
* tf.Operation
  + Represents unit of computation
  + Node of tf.Graph
* tf.Tensor
  + Represents unit of data that flow between operations
  + Edge of tf.Graph








### Step 2. Executing the Computation Graph (Single Machine Case)

* Session object를 사용하여 graph 상의 operation들을 수행
  + Operation의 수행은 OpKernel::Compute()에 정의되어 있음
    - A kernel is a particular implementation of an operation that can be run on a particular type of device (e.g., CPU or GPU)

* Session
  + Lets a caller drive a TensorFlow graph computations
* Executor
  + Runs a graph computation
* Device
  + Actually performs computations

Session -> Executor로 Graph, Executor -> Device로 OpKernel

Session: Graph를 생성하여 나누고 executor에게 분배
Executor 간 동기화를 위해 barrier 사용

Executor: Device들에게 graph 상의 operation의 kernel 수행을 명령

Device: 해당 operation의 kernel 수행

```c
// tensorflow/core/kernels/matmul_op.cc
template <typename Device, typename T, bool USE_CUBLAS> class MatMulOp : public OpKernel {
  public:
  void Compute(OpKernelContext* ctx) override {
    // ...
    LaunchMatMul<Device, T, USE_CUBLAS>::launch(ctx, this, a, b, dim_pair, out);
  }
};
```



```c
// tensorflow/core/kernels/matmul_op.cc
template <typename T>
struct LaunchMatMul<GPUDevice, T, true /* USE_CUBLAS */> {
  static void launch(
    OpKernelContext* ctx, OpKernel* kernel, const Tensor& a, const Tensor& b,
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
