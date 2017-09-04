---
layout: post
title: TensorFlow의 내부 동작에 대한 간단 소개
categories:
  - TensorFlow
---
**아직 작성 중인 글입니다.**

자율주행 자동차와 인공지능과 같은 deep learning을 사용하는 서비스들이 두각을 나타내면서 deep learning 플랫폼에 대한 경쟁도 심화되고 있습니다. 하드웨어 플랫폼의 경우 초창기부터 많은 투자를 한 NVIDIA가 많이 앞서 나가고 있다고 봐도 될 것 같습니다. 특히, NVIDIA의 병렬 컴퓨팅 아키텍처인 CUDA의 deep learning 라이브러리 지원과 개발자 커뮤니티 규모는 경쟁자들을 압도하고 있습니다[^Dettmers17]. 이런 문맥으로 볼 때, Google의 Tensor Processiong Unit(TPU)를 필두로 여러 회사들이 deep learning을 위한 프로세서를 제안하고 있지만 NVIDIA가 쉽게 자리를 내줄지 의문스럽습니다.

반면 소프트웨어 플랫폼은 상대적으로 경쟁이 치열해 보입니다.




### Google TensorFlow의 철학

*
* Jeff Dean
  - Map Reduce 논문의 1저자

> One of the reasons we built TensorFlow, our next-generation system, the system that we’ve actually open sourced for machine learning, is that we wanted to keep the scalable attributes and production readiness of our first system, but make it a much more flexible platform for doing all kinds of machine-learning research and product development [^Jeffrey17]

### TensorFlow 프레임워크의 구조





### TensorFlow 프로그램 구조

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
  train_step = tf.train.GradientDescentOptimizer(0.5).minimize(cross_entropy)

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
  parser.add_argument('--data_dir', type=str, default='/tmp/tensorflow/mnist/input_data',
                      help='Directory for storing input data')
  FLAGS, unparsed = parser.parse_known_args()
  tf.app.run(main=main, argv=[sys.argv[0]] + unparsed)
```

// Expressing computation via Graph과 Executing computation

### Step 1. Expressing Computation via Computation Graph
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

[^Dettmers17]: http://timdettmers.com/2017/04/09/which-gpu-for-deep-learning/
[^Jeffrey17]: Jeffrey Dean and Sanjay Ghemawat, "MapReduce: simplified data processing on large clusters," USENIX Symposium on Operating Systems Design and Implementation (OSDI), 2014.
