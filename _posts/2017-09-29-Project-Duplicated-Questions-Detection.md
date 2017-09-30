---
layout: post
title:  "Duplicated Questions Detection"
date:   2017-09-29
excerpt: "Detecting duplicated questions in Quora"
project: true
tag:
- Tensorflow
- CNN
- RNN
comments: true
---

## Background

In Quora, there are too many questions meaning the same. For example, the two sentence "What is the most populous state in the USA?" and "Which state in the United States has the most people?" are asking the same thing. If duplicated questions are detected, it would be more efficient for user searching answers.

Traditional NLP method can be used for solving this problem, such as cosine similarity and Jaccard similarity \[1\]. In this post a recurrent neural network is described for detecting duplicated questions.

## The Task

Quora issues the dataset \[2\] for duplicated questions detection. Part of the dataset is:

![](https://github.com/ziboyi/Duplicated-Questions-Detection/blob/master/figure/Duplicated-Questions.png?raw=true)

We need a model that can tell whether two questions are duplicated.

## The Model

The architecture of the model is:

![](https://github.com/ziboyi/Duplicated-Questions-Detection/blob/master/figure/arch.png?raw=true)

The remaining part of this section will describle the model layer by layer.

### Embedding Layer

Giving a sentence \\( S=(w_1, w_2,..., w_t) \\), each word will represented as a column vector \\( x_i \\).

### Encoding Layer

![](https://github.com/ziboyi/Duplicated-Questions-Detection/blob/master/figure/Bi-LSTM.png?raw=true)

The bi-LSTM takes the sentence in forward and backward direction, and output the features of each time step. This layer encodes the sentence to a vector \\( r \\).

\\[ r=[F_{h_1}, B_{h_1}, F_{h_2}, B_{h_2},..., F_{h_t}, B_{h_t}] \\]

The following picture shows the LSTM cell.

![](https://github.com/ziboyi/Duplicated-Questions-Detection/blob/master/figure/LSTM-Cell.png?raw=true)

The output of each cell \\( h_i \\) are computed by the following equation,

\\[ \\]
\\[ \\]
\\[ \\]
\\[ \\]
\\[ \\]
\\[ \\]
