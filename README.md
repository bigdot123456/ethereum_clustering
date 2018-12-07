# Clustering Ethereum Addresses

## Categorizing addresses using patterns in transaction activity

### See my medium article[ here](https://towardsdatascience.com/clustering-ethereum-addresses-18aeca61919d)


![](https://cdn-images-1.medium.com/max/800/1*_Ar6rUk6jC9vtDFgq0KdNw.gif)

多图！对4万多个以太坊地址进行聚类，数据可视化后是这样的
2018-12-07 18:26:52 
本文来自网络自动抓取，请自行判别真伪及价值
1_BxydgpDx7w2ZLFB-Ewc6ng

 

介绍

 



以太坊用户可能是匿名的，但他们的地址是唯一标识，在区块链中公开显示。

本文基于以太坊上的交易活动构建了一个分类算法，将以太坊用户划分为不同的行为子组。它可以预测一个地址到底是属于交易所，矿工还是ICO钱包。

数据库使用SQL构建，模型用Python编写。源代码可在GitHub上获得。

WX20181207-145024@2x

以太坊地址特征3D化图示

 

背景

 



以太坊区块链是一个称为智能合约的去中心化应用平台。这些合约通常用于表示其他资产。这些资产可以代表现实世界中的物理对象（如房地产）或纯粹的数字对象（如效用代币）。

执行智能合约所需的计算是用ETH（以太坊生态系统的原生代币）支付的。

ETH则存储在称为地址的加密安全帐户中。

 

为什么要将以太坊地址聚类？

 



许多人认为加密货币为用户提供了数字匿名性，这种想法有一定道理。事实上，匿名性正是门罗币和ZCash的核心使命。

然而，以太坊的使用显然更加广泛，其灵活性也使得以太坊产生了丰富的交易行为公共数据集。由于以太坊地址是所有权不变的唯一标识，因此可以跟踪、汇总和分析用户的交易活动。

在这里，笔者尝试通过有效地分类以太坊地址来创建用户原型。这些原型可用于预测未知地址的所有者。

这为一大批应用程序打开了新的大门：

·了解网络活动

·加强交易策略

·改善反洗钱活动

 

聚类结果

 



以太坊生态系统的参与者可以依据交易活动中的规律分类。根据已知的属于交易所、矿工和ICO项目的地址信息，我们发现聚类的结果大致准确。

WX20181207-145208@2x

 

技术细节

 



以太坊交易数据集托管在Google BigQuery上。针对40,000个拥有最多ETH余额的地址，本次分类创建了25个特征来表征用户行为的差异。

1_Dkza60Nx4wJFG1EqcleEvQ

使用轮廓分析，本次分类确定最佳群集数大致为8，尽量减少了具有负轮廓分数的样本（负分代表样本可能分配给错误的群集）的数量。

1_mHc68oGg5-rndH6ktoeP1Q

通过从Etherscan.io块浏览器中抓取数据，笔者在其数据集中收集了125个地址的众包标签（crowdsourced label）。

大多数标签分为三类：

交易所、矿工和ICO钱包。

聚类是一种无指导的机器学习技术，因此笔者无法使用标签来训练模型。相反，笔者根据每个群集的最高标签密度，使用标签将用户原型分配给群集。结果可以在这里找到。

WX20181207-163349@2x

第一次聚类后的2D视图。左边为已知地址。





重新聚类

一开始，交易所和矿工的地址混在了同一个集群中。为了将它们分离，笔者仅使用该群集中的地址执行第二轮聚类。



通过将不相似度量从欧氏距离改为余弦距离，笔者大大改善了交易所和矿工之间的分离情况。

WX20181207-163545@2x

二次分离。左边为已知交易所和矿工地址

通过将重新聚类的结果替换到原始分析中，我们最终得到9个群集。



WX20181207-163705@2x

二次聚类后的2D视图。左边为已知地址

 

解释结果

 



我们可以基于相应的群集得出关于用户行为的结论。

1_g_Aw8tVTz3_AUSLLPIPg9g

交易所

·ETH余额高



·转入和转出的交易量很高

·交易时间非常不规律

交易所是加密行业的银行。这些结果很直观。

矿工

·ETH余额少



·平均交易规模小

·交易时间更规律

矿工通过消耗算力来验证交易，并获得ETH奖励。矿工经常将他们的算力资源集中起来（矿池）以获得更加平稳的收益，并根据所贡献的资源获取相应收益。

ICO钱包

·ETH余额高



·有少量大额交易

·交易时间最规律

ICO是加密初创公司的常见筹款方法。因此，这些创业公司一般持有大量ETH，并定期出售大量ETH以支付常规业务费用。

其他类别

·交易所和矿工集群非常相似，因为它们是在第二轮集群中创建的。



·群集7中的地址有大量智能合约活动。

·集群2和5有鲜明的区别。

你能识别出这些用户组中的任何一个吗？

1_BsLEsZ9--WaLo7ZJCIcQYQ

1_YKHiXIRxrw5VqtiDGXKT6A

 

后续

 



扩展这项工作可能会让以太坊区块链数据更加细致地呈现。以下是一些特别有趣的方面：

·添加基于图论和网络分析的功能

·将刷单机器人与人类区分开来

·扩展智能合约分析

·重复分析ERC-20代币交易活动

由Goinbit采集分享 
加关注 不定期送好礼
官网： https://www.goinbit.com/
邮箱：Service@GoinBit.Com
电报群: hhttps://t.me/Goinbit
购币网邀请链接 https://www.goinbit.com/m/reg/T66PPD 注册会员即送GBT18个 



ETHEOS

TokenClub
您的区块链投资专家
打开APP
