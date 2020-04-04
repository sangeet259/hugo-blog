+++
draft = false
date = 2019-02-06
title = "Categorical Cross Entropy vs Sparse Categorical Cross Entropy"
slug = "cat-ce-vs-sparse-cat-ce"
tags = ["mldlaiip","keras"]
categories = [ "TIL"]
+++

I was looking for a loss function when browsing through Keras documentation I found two loss functions.
The first one `categorical_cross_entropy` was familiar, however I saw something I had never used before it was `sparsed_categorical_cross_entropy`.

The difference : Depends on the structure of your targets !

If your targets are one-hot encoded, you have to use categorical_crossentropy.
Examples of one-hot encoding:
```
[1,0,0]
[0,1,0]
[0,0,1]
```
But if your targets are integers, use sparse_categorical_crossentropy.
Examples of integer encodings (for the sake of completion):
```
1
2
3
```

**Credits** : https://jovianlin.io/cat-crossentropy-vs-sparse-cat-crossentropy/