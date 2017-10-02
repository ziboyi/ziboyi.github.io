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

<div align=center>
<img src="http://ww2.sinaimg.cn/bmiddle/88070423gw1ep30aw8an7g204d04gkgd.gif" width="" height="" alt="亦菲表演机器猫"/>
</div>



We need a model that can tell whether two questions are duplicated.

## The Model

The architecture of the model is:

![](https://github.com/ziboyi/Duplicated-Questions-Detection/blob/master/figure/arch.png?raw=true)

The remaining part of this section will describle the model layer by layer.

### Embedding Layer

Giving a sentence \\( S=(w_1, w_2,..., w_t) \\), each word will represented as a row vector \\( x_i \\).

### Encoding Layer

![](https://github.com/ziboyi/Duplicated-Questions-Detection/blob/master/figure/Bi-LSTM.png?raw=true)

The bi-LSTM takes the sentence in forward and backward direction, and output the features of each time step. This layer encodes the sentence to a vector \\( r \\).

\\[ r=[F_{h_1}, B_{h_1}, F_{h_2}, B_{h_2},..., F_{h_t}, B_{h_t}] \\]

The following picture shows the LSTM cell.

![](https://github.com/ziboyi/Duplicated-Questions-Detection/blob/master/figure/LSTM-Cell.png?raw=true)

The output of each cell \\( h_i \\) are computed by the following equation,

\\[ f_i=\sigma([h_{i-1},x_i]W_f+b_f) \\]
\\[ i_i=\sigma([h_{i-1},x_i]W_i+b_i) \\]
\\[ g_i={\textrm{tanh}}([h_{i-1},x_i]W_g+b_g) \\]
\\[ c_i=c_{i-1}\otimes{f_i}+i_i\otimes{g_i} \\]
\\[ o_i=\sigma([h_{i-1},x_i]W_o+b_o)\\]
\\[ h_i={\textrm{tanh}}(c_i)\otimes{o_i} \\]

### Multiple Layer Perceptron

The input of MLP is

\\[ p=[r_1,r_2,r_1-r_2,r_1\cdot{r_2}] \\]

and MLP classifies feature vector \\( p \\) as a class that indicate whether the two questions are the same. Class 0 means the two questions are not duplicate, while class 1 means they are duplicate. Let \\( y \\) denotes the class. MLP and \\( y \\) are calculated as follows,

\\[ H_3={\textrm{relu}}(pW_3+b_3) \\]
\\[ H_4={\textrm{relu}}(H_3W_4+b_4) \\]
\\[ s={\textrm{softmax}}(H_4W_5+b_5) \\]

\\[ y={\textrm{argmax}}_i{s_i} \\]

### Training the Model

For the given two sentence, the loss function is:

\\[ l_i=-\sum_{c\in C}{y_{ic}{\textrm{ln}}(s_{ic})} \\]
\\[ l=\frac{1}{N}\sum_{i=1}^N{l_i}=-\frac{1}{N}\sum_{i=1}^N{\sum_{c\in C}{y_{ic}{\textrm{ln}}(s_{ic})}} \\]

\\( y_{ic} \\) means that whether instance \\( i \\) belongs in class \\( c \\). The parameters of this model are as follows.

\\[ \theta=\{W_f,b_f,W_i,b_i,W_g,b_g,W_o,b_o,W_3,b_3,W_4,b_4,W_5,b_5\} \\]

Use optimization method "Adam"[3] to optimaze the loss function. After training, the model can tell whether two given sentences are asking the same thing.

## Experiment

Dataset is split to training and testing set during preprocessing.

![](https://github.com/ziboyi/Duplicated-Questions-Detection/blob/master/figure/preprocessing.png?raw=true)

Next step train this model, please check the hyperparameters.

![](https://github.com/ziboyi/Duplicated-Questions-Detection/blob/master/figure/training.png?raw=true)

Then evaluate this model after training.

![](https://github.com/ziboyi/Duplicated-Questions-Detection/blob/master/figure/eval-RNN.png?raw=true)

I modify the model to CNN version and evaluate it again. I also use Jaccard similarity as the baseline, the evaluation are shown in the following table.

| Model | Accuracy | Precision | Recall | F1 |
|:--------|:-------:|--------:|:-------:|--------:|
| baseline | 0.6657   | 0.5151   | 0.7297 | 0.6039 |
| CNN based model | 0.7911   | 0.6894   | 0.7316 | 0.7098 |
| LSTM based model | 0.8126   | 0.7085   | 0.7874 | 0.7459 |
{: rules="groups"}

## Conclusions

LSTM based model has a better performance in this task.

## References

[1] Jaccard, P. (1901). Étude comparative de la distribution florale dans une portion des Alpes et des Jura. Bulletin de la SociétéVaudoise des Sciences Naturelles 37, 547-579.

[2] https://data.quora.com/First-Quora-Dataset-Release-Question-Pairs, Quora Duplicate Questions Dataset.

[3] Kingma D, Ba J. Adam: A method for stochastic optimization[J]. arXiv preprint arXiv:1412.6980, 2014.
