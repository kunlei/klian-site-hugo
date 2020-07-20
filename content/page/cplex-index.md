---
title: CPLEX
subtitle: 
date: 2019-08-17
draft: false
comments: false
tags: ["cplex", "java"]
---

This is an index page for all CPLEX-related posts.
<!-- [By column]({{< ref "">}}) -->

## CPLEX Java API

1. [Setup]({{< ref "../post/cplex/java/setup.md" >}})
2. API deep dive
   1. [IloAddable and IloModel]({{< ref "/post/cplex/java/iloaddable-and-ilomodel.md" >}})
   2. [IloModel, IloModeler and IloMPModeler]({{< ref "../post/cplex/java/IloModel-IloModeler-IloMPModeler.md">}})
3. Model building
   1. By row
      1. [without IloLPMatrix]({{< ref "../post/cplex/java/build-model-by-row.md">}})
      2. [with IloLPMatirx]({{< ref "../post/cplex/java/build-model-by-row-lpmatrix.md" >}})
   2. By column
      1. [without IloLPMatrix]({{< ref "../post/cplex/java/build-model-by-column.md">}})
      2. [with IloLPMatrix]({{< ref "../post/cplex/java/build-model-by-column-lpmatrix.md" >}})
4. Callback
   1. Optimization callback
      1. [Continuous callback]({{< ref "../post/cplex/java/continuous-callback.md" >}})