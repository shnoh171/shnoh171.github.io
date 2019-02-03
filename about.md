---
layout: page
title: About
sidebar_link: true
---

## Soonhyun Noh

![placeholder](/assets/images/profile.png)

System software engineer, with experience working on operating systems (Linux kernel and AUTOSAR), hypervisors (KVM and XenGT), communication middlewares (DDS), deep learning framework (TensorFlow) and Android framework. Currently interested in platform-level support for edge computing and deep learning (both training and inference).

### Publications

- Soonhyun Noh and Seongsoo Hong, "Splash: A graphical programming framework for an autonomous machine," Submitted to the 16th International Conference on Ubiquitous Robots, 2019.

- Jungho Kim, Philkyue Shin, Soonhyun Noh, Daesik Ham and Seongsoo Hong, "[Reducing memory interference latency of safety-critical applications via memory request throttling and Linux cgroup](http://redwood.snu.ac.kr/wordpress/wp-content/uploads/2018/08/18-07-30-SOCC_Memory-Request-Throttling_final.pdf)," The 31st IEEE International System-on Chip Conference (SOCC), 2018.

- Soonhyun Noh and Seongsoo Hong, "Splash: Stream processing language for autonomous driving," The Workshop on Real-Time and Embedded AI for Autonomous Vehicles (WREAV), 2018.

- Myungsun Kim, Soonhyun Noh, Jinhwa Hyeon and Seongsoo Hong, "[Fair-share scheduling in single-ISA asymmetric multicore architecture via scaled virtual runtime and load redistribution](https://www.sciencedirect.com/science/article/pii/S0743731517302423)," Journal of Parallel and Distributed Computing (JPDC), 2018.

- J. Jose Gonzales E., Chen Luo, Anshumali Shrivastava, Krishna Palem, Yongshik Moon, Soonhyun Noh, Daedong Park and Seongsoo Hong, "[Location detection for navigation using IMUs with a map through coarse-grained machine learning](http://ieeexplore.ieee.org/document/7927040/)," Design, Automation and Test in Europe (DATE), 2017.

- Soonhyun Noh, Myungsun Kim and Seongsoo Hong, "[Enhancing AUTOSAR safety mechanisms for ISO 26262 functional safety requirements](http://redwood.snu.ac.kr/wordpress/paper_server.php?file=eGx4cERzMWVHWUIvSVBKeXUzOG1hWFAyTUZQZ2ZUZVJ3emx4TGJaeWNOWEFCbFRVMnVqbnJyZHVmZ3hVYW40OExSY0lrc1lNS0drQXZXVlJiek5OWWc9PQ==)," FISITA World Automotive Congress, 2016.

- Yongshik Moon\*, Soonhyun Noh\*, Daedong Park\*, Chen Luo\*, Anshumail Shirivastava, Seongsoo Hong and Krishna Palem, "[CaPSuLe: A camera-based positioning system using learning](https://ieeexplore.ieee.org/document/7905476/)," 29th IEEE International System-on Chip Conference (SOCC), 2016. (\* indicates equal contribution)

- Myungsun Kim, Soonhyun Noh, Sungju Huh and Seongsoo Hong, "[Fair-share scheduling for performance-asymmetric multicore architecture via scaled virtual runtime](https://ieeexplore.ieee.org/document/7299846/)," IEEE 21st International Conference on Embedded and Real-Time Computing Systems and Applications (RTCSA), 2015.

### Projects

#### Linux Kernel Techniques for Resource Management

##### Fair-share scheduler for performance-asymmetric multicore architecture
Performance-asymmetric multicore processors have been increasingly adopted in embedded systems due to their architectural benefits in improved performance and power savings. While fair-share scheduling is a crucial kernel service for such applications, it is still at an early stage with respect to performance-asymmetric multicore architecture. We first propose a new fair-share scheduler by adopting the notion of scaled CPU time that reflects the performance asymmetry between different types of cores. Using the scaled CPU time, we revise the virtual runtime of the completely fair scheduler (CFS) of the Linux kernel, and extend it into the scaled virtual runtime (SVR). In addition, we propose an SVR balancing algorithm that bounds the maximum SVR difference of tasks running on the same core types. The SVR balancing algorithm periodically partitions the tasks in the system into task groups and allocates them to the cores in such a way that tasks with smaller SVR receive larger SVR increments and thus proceed more quickly.

##### Dynamic memory resource control for mixed-criticality systems
With the advent of high-performance multicore processors that operate under a limited power budget, dedicated low-end microprocessors with different levels of criticality are rapidly consolidated into a mixed-criticality system. One of the major challenges in designing such a mixed-criticality system is to tightly control the amount of resource contention for a critical application by effectively limiting its performance interference incurred due to sharing resources with non-critical tasks. We propose application-aware dynamic memory request throttling to reduce the memory interference latency of a critical application in a dual criticality system. Our approach carefully differentiates critical task instances from normal task instances and groups them into the critical and normal cgroup, respectively. It then predicts the occurrence of excessive memory contention under critical task execution and then throttles memory requests generated by the normal cgroup via the CPUFreq governor when necessary.

#### In-Depth Kernel Optimization Techniques

##### Predictive on-demand CPU frequency governor on Android smartphones

##### Application context-aware selective packet transmission mechanisms on Android smartphones

#### Software Platform for Real-Time and Embedded AI

##### Splash: A graphical programming framework for an autonomous machine

### Education

- Ph.D. Candidate in Electrical and Computer Engineering, Seoul National University
- B.S. in Electrical and Computer Engineering, Seoul National University
