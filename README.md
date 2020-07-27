# Strabuck
* 这个数据集是一些模拟 Starbucks rewards 移动 app 上用户行为的数据。每隔几天，星巴克会向 app 的用户发送一些推送。这个推送可能仅仅是一条饮品的广告或者是折扣券或 BOGO（买一送一）。一些顾客可能一连几周都收不到任何推送。我的任务是将交易数据、人口统计数据和推送数据结合起来判断哪一类人群会受到某种推送的影响。
## 文件1 Starbucks_Capstone_notebook-01-zh
* 目的：读入数据 对数据进行清洗 整合 特征提取
* 步骤：
  * 读入3个数据
    * portfolio.json – 包括推送的 id 和每个推送的元数据（持续时间、种类等等），
    * profile.json – 每个顾客的人口统计数据，
    * transcript.json – 交易、收到的推送、查看的推送和完成的推送的记录
  * 对类型数据进行onehot编码，把时间数据转化为datetime，分析异常值和缺失值，删除对数据分布没有影响的异常和缺失值，把dict类数据拆成多列，把相似的字符进行统一
  * 将3个处理好的数据集 通过用户id 和 推送offer id，进行整合，得到每个用户的用户信息+交易活动+各活动信息
  * 增加用户特征，比如总交易量、平均交易量、交易次数、推送/完成率等等
  * 
  * 进行数据分析，观察用户特征 如性别，年龄和交易行为之间的关系
  * 



### 2 数据集
### 3 函数集合
### 4 特征工程
## 文件2  Starbucks_Capstone_notebook-01-zh
### 1 简介
* 根据用户数据特征，对用户进行分类
  * 对用户进行PCA处理，找到累计概率超过95%的主成分
  * 采用Kmeans对用户进行分类
  * 对不同分类的用户进行探索性数据分析
### 2 读取数据
### 3 scale
### 4 PCA
### 5 Kmeans分类
### 6 整个流程
## 文件3 Starbucks_Capstone_notebook-03-zh
### 1简介
* 采用特征处理和 含有分类结果的用户数据，进行深度学习，预测用户的推送offer完成率，从而可以针对性的推送offer
  * 首先对数据进行scale预处理，并分割训练集 测试集
  * 通过线性回归初步预测
  * 通过多种算法预测 找到最优算法
  * 对最优算法进行调参
### 2准备数据
### 3机器学习
### 4小结
