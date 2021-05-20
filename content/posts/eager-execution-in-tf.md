+++
draft = false
date = 2019-02-04
title = "Trying out Eager Execution"
slug = "eager-execution-in-tensorflow"
tags = ["mldlaiip","tensorflow"]
categories = ["Tech"]
+++

So there was a Data Science competition and after a long long time, I was doing something in Deep Learning.
I was looking at colab when I saw this term **"Eager Execution"** popping up.

So there I was trying to figure out what it was. Here in this article I try to summarise what I learnt from various sources.

### What it is?

It's a new imperative. The pythonic way of using tensorflow. Earlier you had to build graphs in a session and then run the graph to do the computations. Enabling eager makes `tf.Tensor` objects reference concrete values instead of symbolic handles to nodes in a computational graph.

### Why were there graphs at the first place?

Because graphs are platform indepenedent. You can deploy it better over 100s of machines.

### Why Eager execution?

One doesn't need a static graph and have an apriori view of the computation graph to get the gradients computed. One can also use autograd (what PyTorch does). AutoGrad lets you differentiate dynamic code. You just build up a trace as you go and walk back the trace to compute the gradients.

It helps you analyse and debug your code much better by printing the value of the tensors and inseting breakpoints.

When eager execution is enabled a tensorflow variable is just a python object.

### How to use?

```python
import tensorflow as tf
tf.enable_eager_execution()
```

**NOTE:** `Once eager execution is enabled it cannot be disabled within the same program.`

### Interoperating between graph and eager?

Now since the existing ecosystem in TF is almost entirely graph based, you need way to communicate between eager and graph.

#### Call into graph from eager code:

`tfe.make_template()`

#### Call into eager from graph:

`tfe.py_func()`

### Good Pratices

#### Writing Eager Compatible Code:

- Use `tf.keras.layers`
- use `tf.keras.Model`
- Use `tf.contrib.summary`
- Use `tfe.metrics`
- Use object based saving (enabled with eager, resembles pickling in python)

### Why you should `enable_eager_execution`?

- Being new to ML and having to build graphs in TF can be frustrating, I myself moved form TF to PyTorch for this
- Being a researcher, eager gives one the ability to quickly iterate over models
- Plain Python Debugging
- Profiling your model
