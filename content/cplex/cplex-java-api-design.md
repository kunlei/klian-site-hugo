---
title: Cplex Java API Design
authorbox: true
sidebar: false
draft: false
date: 2019-04-04
categories:
    - "Cplex"
tags:
    - "cplex"
    - "Java"
---

I recently look into how the Java API of Cplex Concert is designed and find that it is a great example of the programming principle "Programming to the Interface, not Implementation".
This post tries to show how various components of this API work together to let us build mathematical models.

### Java Interface Design of Cplex Concert
Let's first take a look of the class diagram, a picture is worth a thousand words, after all.

![test image](/image/cplex-concert-java.png)


