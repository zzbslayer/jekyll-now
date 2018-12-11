---
layout: post
title: Attention
---
## Attention 

This post mainly from [jalammar](https://jalammar.github.io/)

Attention 是在 Neural Translate Model 中使用的一种技巧。

### What is Neural Translate Model(NTM)

NTM 以一个句子输入，输出该句子的翻译结果的 Seq2Seq 的模型。

该模型由 encoder 和 decoder 两层 Recurrent Neural Network(RNN) 组成。

![ntm]({{site.baseurl}}/_posts/image/2018-12-11/ntm.png)

以输入 "我 / 是 / 学生"，输出 "I am a student" 为例。

```
encoder:
    "我" & init hidden state => encoder => hidden state 1
    "是" & hidden state 1 => encoder => hidden state 2
    "学生" & hidden state 2 => encoder => hidden state 3
```

之后将 hidden state 3 输入 decoder 层进行逐个翻译。

### Attention in NTM

而使用了 Attention 技巧以后，并不直接将 hidden state 3 输入。

而是在每次翻译时对于各个 hidden state 进行加权求和输入 decoder 

![attention]({{site.baseurl}}/_posts/image/2018-12-11/attention.png)

还是之前的例子。encoder 层处理完之后，

```
decoder:
	init hidden state => decoder => hidden state 4 
    	=> concatenate(priority_sum(hidden state 1, hidden state 2, hidden state 3), hidden state 4)
        => "I"
    hidden state 4 => decoder => hidden state 5
        => concatenate(priority_sum(hidden state 1, hidden state 2, hidden state 3), hidden state 5)
        => "am"
    hidden state 5 => decoder => hidden state 6
        => concatenate(priority_sum(hidden state 1, hidden state 2, hidden state 3), hidden state 6)
        => "a"
    hidden state 6 => decoder => hidden state 7
        => concatenate(priority_sum(hidden state 1, hidden state 2, hidden state 3), hidden state 7)
        => "student"
```

![attention2]({{site.baseurl}}/_posts/image/2018-12-11/attention2.png)
