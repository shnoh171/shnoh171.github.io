---
layout: post
title: RT Embedded AI를 위한 프로그래밍 언어
categories:
  - Development Supports for AI
---

**현재 작성 중인 포스트입니다.**

딥러닝 기반 AI 기술은 다양한 도메인에서 기존 기술에 비해 월등한 성능을 기반으로 필수 요소로 자리 잡았습니다. 임베디드 시스템에서도 마찬가지입니다. 특히 자율주행자동차나 로봇 등의 도메인에서 AI 기술은 object detection이나 semantic segmentation과 같은 인지 알고리즘 개발에 있어서 대체 불가능한 존재로 부상하였습니다.




자율주행자동차나 로봇 등 다양한 임베디드 시스템에 AI 기술이 도입되고 있습니다.



AI 응용 개발 과정은 기존의 응용 개발 과정과는 매우 다르기 때문에, 임베디드 시스템 상에서 수행될 AI 응용 개발을 위한 지원이 필수적입니다.




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
