---
title: uncategorized
date: 2019-06-25 21:12:35
tags:
---

<!-- more -->

Intel的栈是向下增长的，arm中是可以设置向下或向上增长的。
Intel的ESP指向的是栈顶，不是栈定的下一个位置。EBP指向的空间保存有要return的地址。

stack通常被分成一个一个frame。每个frame包含了传递给另一个函数的local varables, parameters。
stack-frame base pointer(EBP中的内容)

RNN Layer 要求每个batch内数据的time stamp是相同的，如果数据集的time stamp大小不一致，有两个办法，一个就是对每个batch 内作padding。 另一个就是每个数据作单独的batch，一个batch内只有一个数据，那就没有这样的问题了。对于第一种方法，对于padding为0的结尾，还可以用mask来消除影响。

音频处理的一些软件包：

* [ffmpeg](https://ffmpeg.org/)
* [libsndfile](http://www.mega-nerd.com/libsndfile/)
* [PySoundFile](https://pysoundfile.readthedocs.io/en/latest/)
* [librosa](https://librosa.github.io/librosa/)
* [python-sounddevice](https://python-sounddevice.readthedocs.io/en/latest/)
* pygame
* pydub
* pyaudio
* simpleaudio
* winsound
* wavio

音频处理的一些问题：

* 要能够处理以及训练不定长的
* 或许可以研究一下batch_size=1的情况下的训练，就像人一样

numpy 广播机制：

当两个或者多个不同shape的ndarray进行算术运算的时候，会要求它们的shape是兼容(compatiable)的，即每个纬度是兼容的。而两个纬度完全相同，或者其中一个是1，就说这两个维度是兼容的。
维度为1的会扩展到较大的那个维度(通过重复加1，直到相同)。

```plaintext
A      (4d array):  8 x 1 x 6 x 1
B      (3d array):      7 x 1 x 5
Result (4d array):  8 x 7 x 6 x 5
```

上下三角矩阵

numpy.triu
numpy.tril

复制 重复
repeat
tile

机器学习数据保存：
pickle
numpy.save
HDF5(h5py or pytables)
tf record

golang:

package:
一个包的源代码保存在一个或多个以.go为文件后缀名的源文件中。每个包都对应一个独立的名字空间。包还可以让我们通过控制哪些名字是外部可见的来隐藏内部实现信息。在Go语言中，一个简单的规则是：如果一个名字是大写字母开头的，那么该名字是导出的。

tensorflow 使用过程中遇到的问题：

1. loss为nan，常见情况是对0进行log运算等，导致的问题。也有可能是lr 太大导致。
2. keras(tf.keras)提供两种实现model的方法，一种是实现keras.Model的子类，一种是用functional API。前者有一个问题，就是它不能被serialized。因此对于keras.Model的子类而言，`model.inputs, model.outputs, model.to_yaml(), model.to_json, model,get_config(), model.save()`是不可用的。因此对于Subclassed Models，它们是无法安全的serialize的(使用pickle可以，但是是不安全的)。为了加载一个Subclassed Model，必须重新访问代码，并且运行一些数据，然后load_weight。[参考这里](https://tensorflow.google.cn/beta/guide/keras/saving_and_serializing#saving_subclassed_models)。综上，虽然keras.Model的子类更加灵活，但是也存在这些问题，因此推荐使用Functional API来构建。
3. keras layer。对于无状态的操作，推荐使用keras.layers.Lambda，但是对于任何含有trainable weights的，还是需要自行实现layer。

python:

* [module](https://docs.python.org/3/tutorial/modules.html): A module is a file containing Python definitions and statements. 一个py文件就是一个module。python有且只有一种module object。
* package: Packages are a way of structuring Python’s module namespace by using “dotted module names”.  包是python用来组织module名称空间的一种方式。一个含有`__init__.py`文件的文件夹就是一个包。package与module之间的关系，就**好像**目录与文件之间的关系。所有的package都是module，但反过来不成立。换一句话，package就是一种特殊的module。特别的，任何具有`__path__`属性的module就被认为是一个package。所有的module都有名字，子package的名字与父packge的名字之间用一个点来分开。python有两种package。**regular packages**和**namespace package**。regular package通过一个包含有`__init__.py`的文件夹来实现。当一个regular package被import了，`__init__.py`文件被隐式执行，它定义的所有对象被绑定到这个包的名称空间。

```plaintext
parent/
    __init__.py
    one/
        __init__.py
    two/
        __init__.py
    three/
        __init__.py
```

import parent.one 会执行parent/__init__.py和parent/one/__init__.py。后续再import parent.two和parent.three会执行parent/two/__init__.py和parent/three/__init__.py。不会再去执行parent/__init__.py。

* decorator: 装饰器可以用来装饰函数和类。装饰器可以是函数也可以是类。装饰器返回的必须是callable的，无论是函数还是实现了`__call__`方法的类。

```python
@decorator_without_arguments
def fun(b0, b1):
    pass
# same as
def fun(b0, b1):
    pass
fun = decorator_without_arguments(fun)

@decorator_with_arguments(a0, a1, a2)
def fun(b0, b1):
    pass
# same as
def fun(b0, b1):
    pass
fun = decorator_with_arguments(a0, a1, a2)(fun)

```

```python

```

@语法仅仅是一个语法糖。

* [import](https://docs.python.org/3/reference/import.html): 在一个module中的python代码，通过import其他的module，可以访问其他module中的代码。import 包含两个操作：它首先搜寻这个named module，然后它将搜索结果与一个local scope的名称绑定。搜寻操作被定一个为一个对`__import__()`函数的调用。`__import__()`的返回结果用于name binding。当一个module第一次被import，python会搜寻这个module，如果找到了，会创建一个module object，并初始化它。如果没找到，会引发一个ModuleNotFoundError。
首先会从sys.modules中查找。sys.modules是之前所有已经import的module的一个cache，包括中间路径。因此如果`foo.bar.baz`被import了，那么sys.modules就会包含`foo`,`foo.bar`,`foo.bar.baz`。
如果在sys.modules没有找到这个named module。python的import protocol将会被唤醒。这个protocol包含两个对象，`finders`和`loaders`。finder的职责是决定它能否用它所知道的任何策略找到这个module。实现这两个接口的对象被称为`importers`。当它们发现自己能够找到module时，就会返回自己。
python有一些默认的`finders`和`importers`。第一个就是知道如何定位built-in modules的。第二个知道如何定位frozen moduels。第三个从`import path`寻找modules。import path是一个列表，包含了系统的文件路径，或者zip文件。它也能扩展到任何可以定位的资源，比如那些被URL所标识的资源。
import机制是可扩展的，因此能添加新的finder来增加module search的scope。
Finder并不加载module。如果它们找到了module，它们会返回一个*module spec*。这是一个与module import相关信息的一个封装。
import机制的可扩展性的主要机制就是`import hooks`。有两种import hooks:`meta hooks`和`import path hooks`。 meta hooks在import开始处理的时候就被调用，在任何其他import操作前就发生了。除了向sys.modules中查找。这允许meta hooks覆盖sys.path操作，frozen modules 甚至 built-in moduels。 meta hook通过向sys.meta_path增加一个新的finder object来注册。
import path hooks左右sys.path(或者package.__path__)处理过程的一部分来调用。Import path hooks 通过向sys.path_hooks添加新的callables来注册。
当module无法在sys.modules中发现，python接着在sys.meat_path搜索，它包含了一系列的meta path finder object。这些finder以此查询，看它们是否能够处理这个named module。
如果sys.meta_path到最后没有返回一个module spec，那么会发生一个ModuleNotFoundError。
如果module spec已经发现，import机制会在加载module时使用它。
如果在sys.modules中已经存在这个module，那么就会返回这个module。
module将在loader执行module代码前就存于sys.modules。这能防止module递归的自我调用。
如果loading失败，有且仅仅是这个失败的module会从sys.modules中移除。其他所有成功加载的module，即使是因为一些副作用而加载的，也都必须保留在sys.modules中。
在module被创建，但是在执行前，import机制将会设置与import有关的属性。
module执行是一个关键步骤，在这个时候，module的namespace将被填充。执行是完全授权于loader的，由loader来决定什么被填充以及如何。
在loading中被创建的module以及被传递给`exec_module()`的module可能不会是import最后所返回的。
module loader提供了loading中最关键的功能： module execution。import 机制调用importlib.abc.Loader.exec_module()方法，和一个参数，就是要执行的module。exec_module()的任何返回值都会被忽略。
loaders必须满足一些条件：
如果module是一个python module（不同于built-in module 和动态加载的扩展），loader必须执行在module的global name space(moduel.__dict__)中的代码。
如果loader无法执行module，那么它应该引发ImportError。
在很多情况，finder和loader可以是同一个对象。

当一个submodule被加载了，parent module的namespace中会有一个binding到submodle对象。

```plaintext
spam/
    __init__.py
    foo.py
    bar.py
```

在import spam.foo后，spam有一个foo属性，绑定到其子模块。

module spec 最终以module的一个__spec__暴露出来。

import机制将会填充module obejct的以下属性。
__name__
__loader__
__package__
__spec__
__path__
__file__
__cached__

定义上来说一个有__path__属性的module就是一个package。这个属性用来import它的subpackages。

__main__ module在python解释器一开始就被初始化了，就像sys和builtins。

如何保存二进制文件(文档、音频、视频)：
