---
title: gnn-parallel-communication_neural_network-read
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-30 20:46:40
tags:
- gnn-parallel
- cnn
- paper
categories:
- [gnn-parallel, cnn]
---

## 1. Abstract

a simple neural model, CommNet,  continuous communication for fully cooperative task.

!!! can according to the language devised by the agents to do effective strategies.

## 2. Introduction

1. each actor, limited capabilities or visibility.

2. robot soccer. main methods is RL,, but need specification and format of the communication is pre-determined. !

3. paper:
    - agent: deep feed-forward network, a continuous vector, and communicate with others.
    - transmission is not a-prioir?
    - involving partial visibility of the enviornment
    - allows dynamic variation of the number and type of agents
    - targets
        - J agents,  maximize reward R
        - receive R is independent of their contribution ? example
        - f-f nn:  inputs -> actions   single ???
    - two cases
        - cnn
        - rl:   material