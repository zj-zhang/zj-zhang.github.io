---
layout: post
title:  "Installing Tensorflow with CUDA 8.0"
categories: Programming
tags: programming python tensorflow
author: ZZJ
academia: true
mathjax: false
---

Tensorflow is arguably the most popular deep-learning framework nowadays. Want to install and check it out? Sure! just a few google clicks away.

However, sometimes you will have an outdated CUDA installed in your group or your company's GPU server that is no longer valid in most online tutorials (as of now, CUDA 9.0 is in every doc), but do not have the permission to update it. If you encounter cases like the one I am facing now, you will need to find an old source to install Tensorflow.



* content
{:toc}

Here is a working solution to install Tensorflow(=1.1.0) on a GPU with CUDA 8.0, on a Tesla K40m GPU server. I was using Anaconda3 with python 3.6.5.

## 1. Create a new virtual environment.

Create a new virtual environment in Anaconda so it does not mess with your other installed packages.

```
$ conda create -n tf pip python=3.6.5 # do NOT name your env 'tensorflow', as it is confused with the package

$ source activate tf
(tf)$ # Your prompt should change
```

## 2. Install the RIGHT version of Tensorflow. 

For more info, see here http://tflearn.org/installation/#tensorflow-installation.
```
# Ubuntu/Linux 64-bit, GPU enabled, Python 3.6
# Requires CUDA toolkit 8.0 and CuDNN v5. For other versions, see "Installing from sources" below.
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.1.0-cp36-cp36m-linux_x86_64.whl
$
$ pip install --ignore-installed --upgrade $TF_BINARY_URL
```

## 3. Test your installation. 

First open "python" in your terminal, then type:
```
# Python
import tensorflow as tf
hello = tf.constant('Hello, TensorFlow!')
sess = tf.Session()
print(sess.run(hello))
```

## 4. Configure TF GPU memory.
You will also want to limit your Tensorflow GPU usage to one or two GPUs, just so other users will not get mad.. Add the following two lines to your ".bash_profile" file:

```
export CUDA_DEVICE_ORDER="PCI_BUS_ID"

export CUDA_VISIBLE_DEVICES=3  # only use GPU number 3
```