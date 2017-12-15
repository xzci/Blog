# 趋势处理

# NLP典型应用

* junk Mail Filter
* Sentiment Analysis
* Input Prediction
* Chatbot
* Automatic summarization
* Speech recognition
* ...

# 常用模型和工具

* Parsing Tree  单纯分割(形式化，结构化的，经常在编程中使用)
* N-Gram  单纯分割-取得多次重复的组合
* WordzVec 取得一些相关性
* Logistic Regression 
* Markov Chain  _很有用_ 可解释性高
* Hidden Markov Model
* Bayesian
* Neural Net 可解释性低（黑盒化）


## 常用方法 
* 词库 （没有结构关系，多数情况下较为有效和常用）

先验知识对NLP的影响：
NLP没有先验知识，所以对省略主语的情况或者明显夸的形容词无法做出正确的反应。
**一种解决方案解决一种场景**

通过分析政经新闻，自动交易。（规避损失）NLP是系统的一部分。后面的统计系统进行计算（神经网络）


### 展开
神经网络NLP的后端或者是相关的应用
    1. Domain Knowledge（**不知道提出什么问题**）
    2. Data （脏数据和噪音可能会对一个正确的模型做出不好的影响）
    3. Models (很多已经有了实现)
    重要性从上向下。

需要对行业进行相关的理解
1. 没有项目
2. 噪音影响结果
3. 满足前2样并且都比较相似的时候，才会是比较决定的因素
预测要有多样性，防止数量大的群体相信该预测，跟风，导致预测失败。

Domain knowledge 获取相关的参数

数据只需要到达可以得出结果的范围的时候就可以称之为大数据。
现行的Models可能不是通过数据训练得到的（源比较少的时候）

### 趋势
更加自动化，智能化

#### 练习
从简单的开始。NLP
