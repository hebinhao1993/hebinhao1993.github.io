---
title: nlp
date: 2019-09-10 13:37:40
tags:
---

<!-- more -->

参考： <https://www.zhihu.com/question/335289475>

文本分类

文本匹配

对话/问答/推荐

对数据挖掘感兴趣：推荐，反作弊，风控等场景下的nlp问题。

对检索系统或者问答对话感兴趣的话，重点了解一下检索士问答（检索+排序的框架就好，不用着急碰机器阅读理解问题）、检索式闲聊机器人，FAQ系统怎么做；还可以简单了解一下任务型对话系统和机器阅读理解，但是这两个问题不建议一个人瞎搞。

文本分类： bow, ngram, tfidf这些基本概念，了解下nlp里面常用的特征，配合NB，SVM能轻松体验特征工程解决文本分类的基本套路；熟悉深度学习框架后，textcnn，fasttext，adasent，dpcnn甚至bert都可以拿来跑一下，研究一下代码。

匹配任务，花式attention，区分下pointwise，pairwise甚至listwise的选炼方式，提高写复杂网络和调参的能力。尤其可以跑实验感受一下query-query匹配与query-response匹配有什么不同。

序列标注问题里的CRF是什么，机器翻译里提出的transformer的设计细节，语义表示相关的ELMo、bert等为什么work，生成相关的VAE、copy机制等在解决什么问题，以及对抗样本，多任务学习，迁移学习、FSL、NAS、灾难性遗忘与持续学习等ML问题等，有条件可以熟悉一下多机多卡训练。

过去三年的ACL、EMNLP、NAACL论文。做好笔记。

文本分类、情感分析(判别式任务)
生成式任务

fasttext, textCNN, textRNN, RCNN, CharCNN, Seq2seq with attention, transformer

自然语言处理怎么最快入门？
<https://www.zhihu.com/question/19895141>

自然语言处理NLP技术里程碑、知识结构、研究方向和机构导师
<https://zhuanlan.zhihu.com/p/47438065>
