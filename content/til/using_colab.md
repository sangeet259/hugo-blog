+++
draft = false
date = 2019-01-21
title = "Accessing Files in Colab"
slug = "files-in-colab"
tags = ["data","mldlaiip","gpu","jupyter","colab"]
categories = [ "TIL"]
+++

To access your Google Drive files in Colab. You first need to mount it in your notebook.

Here's how to do that:

```python
from google.colab import drive
drive.mount('/content/drive')
```

After this you would be asked for authorisation and provided with a token, copy and paste that token back to mount your notebook at `/content/drive/ My Drive`.

From this point onward you can use `cd` and `ls` to explore your drive and go where the data is.