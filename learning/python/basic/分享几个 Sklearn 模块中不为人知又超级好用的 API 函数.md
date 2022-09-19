# 分享几个 Sklearn 模块中不为人知又超级好用的 API 函数

<a id="profileBt"></a><a id="js_name"></a>AI科技大本营 *2022-04-21 18:15*

The following article is from 关于数据分析与可视化 Author 俊欣

<a id="copyright_info"></a>[![](../../../_resources/0_ce134b2de8b34999bb90d45ae7817bd9.jpg)<br>**关于数据分析与可视化** .<br>本公众号定期分享数据分析与可视化干货文章，并有时结合热点话题进行深入讨论，希望您会喜欢，要是哪里写的不好，也渴望倾听您的想法和意见，感谢！❤️](#)

<img width="657" height="116" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__70ffaead029e4926a.gif"/>

作者 | 俊欣

来源 | 关于数据分析与可视化

相信对于不少机器学习的爱好者来说，训练模型、验证模型的性能等等用的一般都是`sklearn`模块中的一些函数方法，今天小编来和大家聊一下该模块中那些不那么为人所知的`API`，可能知道的人不多，但是十分的好用。

极值检测

数据集当中存在着极值，这个是很正常的现象，市面上也有很多检测极值的算法，而`sklearn`中的`EllipticalEnvelope`算法值得一试，它特别擅长在满足正态分布的数据集当中检测极值，代码如下

```


import numpy as np
from sklearn.covariance import EllipticEnvelope
# 随机生成一些假数据
X = np.random.normal(loc=5, scale=2, size=100).reshape(-1, 1)
# 拟合数据
ee = EllipticEnvelope(random_state=0)
_ = ee.fit(X)
# 新建测试集
test = np.array([6, 8, 30, 4, 5, 6, 10, 15, 30, 3]).reshape(-1, 1)
# 预测哪些是极值
ee.predict(test)



```

output

```


array([ 1,  1, -1,  1,  1,  1, -1, -1, -1,  1])



```

在预测出来哪些数据是极值的结果当中，结果中“-1”对应的是极值，也就是30、10、15、30这些结果

特征筛选(RFE)

在建立模型当中，我们筛选出重要的特征，对于**降低过拟合的风险**以及**降低模型的复杂度**都有着很大的帮助。`Sklearn`模块当中递归式特征消除的算法(RFE)可以非常有效地实现上述的目的，它的主要思想是通过学习器返回的`coef_`属性或者是`feature_importance_`属性来获得每个特征的重要程度。然后从当前的特征集合中移除最不重要的特征。在剩下的特征集合中**不断地重复递归这个步骤**，直到最终达到所需要的特征数量为止。

我们来看一下下面这段示例代码

```


from sklearn.datasets import make_regression
from sklearn.feature_selection import RFECV
from sklearn.linear_model import Ridge
# 随机生成一些假数据
X, y = make_regression(n_samples=10000, n_features=20, n_informative=10)
# 新建学习器
rfecv = RFECV(estimator=Ridge(), cv=5)
_ = rfecv.fit(X, y)
rfecv.transform(X).shape



```

output

```


(10000, 10)



```

我们以`Ridge()`回归算法为学习器，通过交叉验证的方式在数据集中去掉了10个冗余的特征，将其他重要的特征保留了下来。

决策树的绘制

相信对不少机器学习的爱好者来说，决策树算法是再熟悉不过的了，要是我们同时能够将其绘制成图表，就可以更加直观的理解它的原理与脉络，我们来看一下下面的这个示例代码

```


from sklearn.datasets import load_iris
from sklearn.tree import DecisionTreeClassifier, plot_tree
import matplotlib.pyplot as plt
%matplotlib inline
# 新建数据集，用决策树算法来进行拟合训练
df = load_iris()
X, y = iris.data, iris.target
clf = DecisionTreeClassifier()
clf = clf.fit(X, y)
# 绘制图表
plt.figure(figsize=(12, 8), dpi=200)
plot_tree(clf, feature_names=df.feature_names, 
               class_names=df.target_names);



```

output

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

HuberRegressor回归

数据集当中要是存在极值会大大降低最后训练出来模型的性能，大多数的情况下，我们是通过可以通过一些算法来找到这些极值然后将其去除掉，当然这里还有介绍的`HuberRegressor`回归算法给我们提供了另外一个思路，它对于极值的处理方式是在训练拟合的时候给予这些极值**较少的权重**，当中的`epsilon`参数来控制应当是被视为是极值的数量，**值越小说明对极值的鲁棒性就越强**。具体请看下图

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

当`epsilon`的值等于1.35、1.5以及1.75的时候，受到极值的干扰都比较小。该算法具体的使用方法以及参数的说明可以参照其官方文档。

https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.HuberRegressor.html

特征筛选 SelectFromModel

另外一种特征筛选的算法是`SelectFromModel`，和上述提到的递归式特征消除法来筛选特征不同的是，它在数据量较大的情况下应用的比较多因为它有**更低的计算成本**，只要模型中带有`feature_importance_`属性或者是`coef_`属性都可以和`SelectFromModel`算法兼容，示例代码如下

```


from sklearn.feature_selection import SelectFromModel
from sklearn.ensemble import ExtraTreesRegressor
# 随机生成一些假数据
X, y = make_regression(n_samples=int(1e4), n_features=50, n_informative=15)
# 初始化模型
selector = SelectFromModel(estimator=ExtraTreesRegressor()).fit(X, y)
# 筛选出重要的模型
selector.transform(X).shape



```

output

```


(10000, 9)


```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

往

期

回

顾

技术

[强大的Gensim库用于NLP文本分析](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247559004&idx=2&sn=f9d719bf98af36db9bdd7e022525ae5b&chksm=cfbb0232f8cc8b24bb0caca20a8818db48b4ff07b7d4abbd087360675c1afe375623560544b0&scene=21#wechat_redirect)

资讯

[何同学又上热搜了，这次为什么？](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247558949&idx=2&sn=f4192d10906f4162e704bd27ca40d3ba&chksm=cfbb024bf8cc8b5d5d2a22434a1b0f1b55e2bae90d227f80eb2bb66f007fd6f9ec28ecf4d296&scene=21#wechat_redirect)

技术

[介绍一个效率爆表的探索性插件](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247559004&idx=1&sn=257df5badfd76a52c390ea967c469b26&chksm=cfbb0232f8cc8b2450cc61d80a8279ed3810438904fa43a4515cbd499fbe53ff1add9a4efc51&scene=21#wechat_redirect)

技术

[用Python打造一个语音合成系统](http://mp.weixin.qq.com/s?__biz=Mzg4NDQwNTI0OQ==&mid=2247558399&idx=1&sn=f7591eaea3f790e6fe7e06e5675f847f&chksm=cfbb0191f8cc88873e2ecdec59b33deb52555e4538fdea7be61ed3cac7c612ac5122428c3c74&scene=21#wechat_redirect)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**分享**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**点收藏**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**点点赞**

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**点在看**

People who liked this content also liked

一次建模，终身受益③

...

六诚啊

不看的原因

- 内容质量低
- 不看此公众号

CNS级宝藏公众号：R语言从入门��大师！

...

R语言数据分析指南

不看的原因

- 内容质量低
- 不看此公众号

奇妙的 CSS MASK

...

iCSS前端趣闻

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___adf2c4457d1845a49.bmp"/>

Scan to Follow