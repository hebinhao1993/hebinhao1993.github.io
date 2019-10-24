---
title: keras.md
date: 2019-05-28 10:22:46
tags:
---

以下代码为例子

```py
from keras.layers import Input, Dense
from keras.models import Model

# This returns a tensor
inputs = Input(shape=(784,))

# a layer instance is callable on a tensor, and returns a tensor
x = Dense(64, activation='relu')(inputs)
x = Dense(64, activation='relu')(x)
predictions = Dense(10, activation='softmax')(x)

# This creates a model that includes
# the Input layer and three Dense layers
model = Model(inputs=inputs, outputs=predictions)
model.compile(optimizer='rmsprop',
              loss='categorical_crossentropy',
              metrics=['accuracy'])
model.fit(data, labels)  # starts training
```

### 核心概念

#### tensor

以tensorflow为backend，一个keras_tensor就是在tensorflow中的tensor，添加了一些attributes，比如_keras_shape，_keras_history等

#### node

node表示多个layer之间的连接关系

#### layer

layer主要表示某个计算步骤

#### network

network继承自layer
network是一个layer的有向无环图

#### model

model继承自netwok，model只是在netwok的基础上添加了一些训练相关的方法。