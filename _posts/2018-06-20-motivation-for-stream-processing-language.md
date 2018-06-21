---
layout: post
title: RT Embedded AI를 위한 프로그래밍 언어
categories:
  - Development Supports for AI
---

**현재 작성 중인 포스트입니다.**

자율주행자동차나 로봇 등의 임베디드 시스템에서 딥러닝 기반 AI 기술이 필수 요소로 자리잡았습니다. AI 응용의 개발은 아래 그림과 같이 end-to-end approach와 modular approach로 분류할 수 있습니다. End-to-end approach는 하나의 거대한 neural network를 사용하여 입력에 대한 출력을 바로 얻어내는 방법입니다. 반면 modular approach는 응용을 기능에 따라 여러 개의 module로 나누고, 각 module에 필요에 따라 AI 기술을 적용하는 방식입니다. 혹자는 end-to-end approach을 지향하여야 한다고 주장하지만, (1) 응용 내에 딥러닝이 효과적이지 않은 부분이 있을 수 있고 (2) 학습해야하는 파라미터의 수가 너무 방대해지며 (3) 예외 상황에 대한 대처가 쉽지 않다는 점에서 한계가 있습니다. 이에 따라 현재 대부분의 회사/연구소들은 modular approach를 채택하고 있고, 이 경향이 계속될 전망입니다.

![placeholder](https://i.imgur.com/Gatb5Qo.png "Figure 1")
*Figure 1. End-to-end Approach vs Modular Approach in Autonomous Driving*

AI 응용의 개발 과정은 기존의 임베디드 시스템 상의 응용 개발 과정과는 매우 다르기 때문에 이에 대한 지원이 필수적입니다. 임베디드 AI 응용 개발은 앞서 언급했듯이 modular approach를 따르기 때문에 크게 programming small과 programming large로 나눌 수 있습니다. Programming small은 각 module의 개발을 의미합니다.







- Embedded AI 응용의 프로그래밍은 크게 programming large와 programming small로 나눌 수 있다.
- Programming small은 많은 deep learning framework들에서 지원한다.
- 하지만 programming large에 대한 지원은 현재 미비하다.
  - 대표적인 도메인인 자율주행의 경우, (1) 플랫폼 지원 없이 C/C++ 등의 언어로 바로 개발하거나, (2) 기존의 ROS, Simulink, AUTOSAR 등에서 제공하는 개발 방법론을 하나 이상 활용한다.
    - Waymo/Tesla/Drive.ai/Uber/Apple?
  - 하지만 이런 개발 방법론들은 embedded AI의 핵심 요구사항인 real-time stream processing을 거의 고려하지 못한다.
- Real-time stream processing을 위해서는 다섯 가지 세부적인 요구사항들을 만족시켜야 한다.
  1. Visual programming
  2. Timing annotation
  3. Exception handling
  4. Support for sensor fusion
  5. Support for multiple modes
  6. Integration support
- 기존에 stream processing을 위한 programming language나 library들이 많이 제안되었지만, 이들은 대부분 cloud computing의 분산 처리를 대상으로 설계되었기 때문에 적절하지 않다.





[^BMW15]: <https://roscon.ros.org/2015/presentations/ROSCon-Automated-Driving.pdf>
