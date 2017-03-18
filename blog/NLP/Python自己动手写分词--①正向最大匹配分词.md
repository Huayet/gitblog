<!--
author: 孙华琛
date: 2017-02-03
title: Python自己动手写分词 — ①正向最大匹配分词
tags: NLP
category: NLP
status: publish
summary: Python自己动手写分词 — ①正向最大匹配分词
-->
# Python自己动手写分词 — ①正向最大匹配分词
---

本系列参考了[哈工大NLP入门训练代码库](https://github.com/HIT-SCIR/scir-training-day)，已fork到[自己的库](https://github.com/Huayet/scir-training-day)。

## 目录

> * **正向最大匹配分词**
> * 逆向最大匹配分词
> * 双向最大匹配分词
> * 其他待定，参考哈工大训练题

------

## 参考资料
* [1] [数学之美系列二 谈谈中文分词](http://product.dangdang.com/product.aspx?product_id=22754640)
* [2] [漫话中文自动分词和语义识别（上）：中文分词算法](http://www.matrix67.com/blog/archives/4212)
* [3] [中文分词入门之最大匹配法](http://www.52nlp.cn/maximum-matching-method-of-chinese-word-segmentation)
* [4] [中文分词基础原则及正向最大匹配法、逆向最大匹配法、双向最大匹配法的分析](http://blog.sina.com.cn/s/blog_53daccf401011t74.html)

------
## PS:分词算法设计中的几个基本原则[[4]](http://blog.sina.com.cn/s/blog_53daccf401011t74.html)
1. 颗粒度越大越好：用于进行语义分析的文本分词，要求分词结果的颗粒度越大，即**单词的字数越多**，所能表示的含义越确切，如：“公安局长”可以分为“公安 局长”、“公安局 长”、“公安局长”都算对，但是要用于语义分析，则“公安局长”的分词结果最好（当然前提是所使用的词典中有这个词）
2. 切分结果中**非词典词越少越好**，单字字典词数越少越好，这里的“非词典词”就是不包含在词典中的单字，而“单字字典词”指的是可以独立运用的单字，如“的”、“了”、“和”、“你”、“我”、“他”。例如：“技术和服务”，可以分为“技术 和服 务”以及“技术 和 服务”，但“务”字无法独立成词（即词典中没有），但“和”字可以单独成词（词典中要包含），因此“技术 和服 务”有1个非词典词，而“技术 和 服务”有0个非词典词，因此选用后者。
3. **总体词数越少越好**，在相同字数的情况下，总词数越少，说明语义单元越少，那么相对的单个语义单元的权重会越大，因此准确性会越高。

---
## 文本预切分
如果输入是整段文本，可以先把输入按标点符号为边界进行切分。
```python

# -*- coding: utf-8 -*
# 输入文本
text = '在数据抓取完成后，你拿到的数据往往是一篇文章或者一大段文字。在进行其他处理之前，你需要先对文章进行切割或者处理(去除多余字符、特殊符号、分句和分词)'
# 切分符号 [。，,！……!《》<>\"':：？\?、\|“”‘’；]{}（）{}【】()｛｝（）：？！。，;、~——+％%`:“”＂'‘\n\r
cutList = ['。','，','？','！']
# 预切分函数
def cutText(text,cutList):
    cutResult = []
    cutResult.append(text)
    for c in cutlist:
        textList = cutResult
        cutResult = []
        for text in textList:
            splitResult = text.split(c)
            for s in splitResult:
                cutResult.append(s)
    return cutResult
# 测试函数
cutResult = cutText(text,cutlist)
for t in cutResult:
    print t
    
```