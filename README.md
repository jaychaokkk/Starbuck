# Strabuck
* 这个数据集是一些模拟 Starbucks rewards 移动 app 上用户行为的数据。每隔几天，星巴克会向 app 的用户发送一些推送。这个推送可能仅仅是一条饮品的广告或者是折扣券或 BOGO（买一送一）。一些顾客可能一连几周都收不到任何推送。我的任务是将交易数据、人口统计数据和推送数据结合起来判断哪一类人群会受到某种推送的影响。
* 用到的库 pandas，numpy，math，json, seaborn, matplotlib, tqdm, datetime, pickle, sklearn,warnings
  
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
  * 根据每个用户的推送活动id 和活动信息，判断该推送有效时间以外是否成功交易，记为invalid=0(有效交易) or 1(无效交易)
  * 进行数据分析，观察用户特征 如性别，年龄和交易行为之间的关系
* 结果：
  * 对于 profile数据，发现118岁的用户有2175名，占总数据的14%，年龄值异常，并且在income和gender上都缺失了信息，在通过特征工程拓展特征以后，缺失和无缺失的分布基本一致，可以删除缺失行。
  * 对于 transcript数据，value 列有两种情况 'offer id' 或者'offer_id' 字典，值为offerid，'amount'字典，值为交易量，因此这一列需要拆成2列，分别用'offer_id'和'amount'作为列名，值为对应的字典值，注意需要把'offer id'变为'offer_id'
  * 对于bogo_5_7_5和discount_20_10_5这两种推送，complete 竟然比 view还多 说明有的人 complete 和推送无关, 属于无效记录。排除无效记录后，对所有推送均有 complete 比 view 少，情况合理。
  
## 文件2  Starbucks_Capstone_notebook-01-zh
* 用到的库 pandas，numpy，math，json, seaborn, matplotlib, tqdm, datetime, pickle
* 目的：根据用户数据特征，对用户进行分类
  * 对用户进行scale和 PCA处理，找到累计概率超过95%的主成分
  * 采用Kmeans对用户进行分类
  * 对不同分类的用户进行探索性数据分析
* 结果：
  * Kmeans曲线的Silhouette值在12的时候最高，Elbow曲线并没有明显拐点，因此用户分类数选择12
  * 8 3 10 这几类客户最多，其次是0 1 2 4 6 这5类，5 7 9 11 这四种客户比较少
  * 0 6 10 这三类用户的男性最多，2 3 8 这三类用户的女性最多， 所有的未知性别的人都在第9类用户里
  * complete/recieve 比值，和complete/recieve 比值 都是1类用户最高，其次是6 9 11类用户，0 3 8 10这四类用户的 offer完成率比较差
  * 11类用户的收入和平均消费都远高于其他类别， 0 6 10 11 这四类用户年纪比较大，3 5 8 这三类用户的年纪比较小
  
## 文件3 Starbucks_Capstone_notebook-03-zh
* 目的：
  * 采用特征处理和 含有分类结果的用户数据，进行深度学习，预测用户的推送offer完成率，从而可以针对性的推送offer
  * 首先对数据进行scale预处理，并分割训练集 测试集
  * 通过线性回归初步预测
  * 通过多种算法预测 找到最优算法
  * 对最优算法进行调参
* 结果：
  * 对该数据最好的3种算法 依次是 GradientBoostingRegressor，BaggingRegressor，RandomForestRegressor
  * 采用线性回归方法 得分 R^2:0.4084 MSE:0.0633 ，采用Gradient boosting方法 得分 R^2: 0.4925 MSE: 0.0546， 采用调参后的Gradient boosting方法 得分 R^2 score:0.4899 MSE:0.0546
  * 最终模型准确率并不高，可能是因为特征提取的还不够 需要更多特征 比如提取用户 对每一种推送offer接收数量、完成数量和完成/接收率，以及提取用户的年龄分组
