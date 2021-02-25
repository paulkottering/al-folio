---
layout: page
title: RL-QAOA Research
description: A summary of my research contributions to Professor Lin's group.
img: /assets/img/0.png
importance: 3
---

Since July 2020 I have been working with Professor L. Lin’s group in the Department of Applied Mathematics at UC Berkeley, where our research group has sought to improve the Quantum Approximate Optimization Algorithm (QAOA) by using deep reinforcement learning (RL). Leveraging the flexibility and efficiency of a deep autoregressive neural network we were able to demonstrate accuracy gains over prior quantum control methods. The enhanced method mixes the uncertainty-resilient policy gradient protocol optimizer and the complex combinatorial task of constructing a sequence of unitaries out of a predefined expanded set of gates, based on counter-diabatic driving, through a novel algorithm called RL-QAOA. Please see our [paper](/assets/pdf/2012.06701.pdf){:target="\_blank"} for greater understanding of the project.

Originally tasked with adapting the standard Policy Gradient QAOA (PG-QAOA) through the introduction of a time duration constraint, I developed with multiple implementation approaches such as resampling, introducing additional RL penalties and a normalization procedure. After implementing these variants in TensorFlow I communicated and re-iterated with the team; carrying out a systematic comparison of the options, and eventually finalizing a strategy based on our findings. Utilizing physically-motivated intuition I then studied how initialization of the protocol distribution affected the performance of the PG-QAOA and RL-QAOA, and  provided visualizations of the distribution’s temporal evolution. Finally, through my performance analysis of a beta distribution instead of the initially-chosen sigmoid Gaussian distribution, I showed the former to be more versatile and reach higher energy. Through these contributions I developed a better, more compactly-supported policy and helped improve the performance of the algorithm.

By graduating this Winter I will be able to further explore scientific machine learning by working full time with Professor Lin during the Spring 2021 semester. This experience will provide me with additional research experience that will smooth my transition to postgraduate education.

I am very grateful for all the mentorship and explanations that [Jiahao Yao](https://jiahaoyao.github.io/){:target="\_blank"} has given me during this research experience.
