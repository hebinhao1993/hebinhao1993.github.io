---
title: tf-dataset.md
date: 2019-05-27 20:49:20
tags:
---

## tensorflow Dataset

`tf.data.Dataset`表示一系列元素，其中每个元素包含一个或多个 Tensor 对象。

`Dataset.map()`

`Dataset.repeat()`

`Dataset.batch()`

`Dataset.shuffle()`

`tf.data.Iterator`提供了从数据集中提取元素的主要方法。`Iterator.get_next()`返回的操作会在执行时生成 Dataset 的下一个元素，并且此操作通常充当输入管道代码和模型之间的接口。\
