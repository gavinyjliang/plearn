# 又一个超参数优化神器：Scikit Optimize

<a id="copyright_logo"></a>Original 云朵君 <a id="profileBt"></a><a id="js_name"></a>数据STUDIO *2022-05-16 08:35* *Posted on <a id="js_ip_wording"></a>四川*

![Image](../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__bdb04d8031e44a38b.gif)

<a id="js_appmsg_search_keywords_head"></a>![](../../../_resources/0_wx_fmt_png_3164a3a109fe498db7098cba42ed385c.png)<a id="appmsg_search_keywords_nickname"></a>数据STUDIO<a id="appmsg_search_keywords_title"></a>推荐搜索<a id="appmsg_search_keywords_tips"></a>关键词列表：<a id="appmsg_search_keywords_keywords_list"></a>Python机器学习数据挖掘可视化

<img width="677" height="103" src="../../../_resources/640_wx_fmt_gif_wxfrom_5_wx_lazy__2b134bd068c64260b.gif"/>

##### 本文中，云朵君将和大家一起学习另一个超参数优化神器：skopt，并从 易用性、搜索空间、优化方法、可视化等方面简单介绍 skopt，最后使用 skopt 对实际问题运用贝叶斯超参数优化的示例。

开始之前，请问你是不是考虑执行贝叶斯超参数优化，但又不确定如何操作？听说过各种超参数优化库，如前两次介绍的[模型调参神器：Hyperopt](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247523832&idx=1&sn=b644a9e82bd882a60088a01697933ad9&chksm=c359c052f42e4944b5f05e484d84707859974d5b8011707a93af93d7cf566a5245fbae9bd99d&scene=21#wechat_redirect) ｜ [使用 Hyperopt 和 Plotly 可视化超参数优化](http://mp.weixin.qq.com/s?__biz=Mzk0OTI1OTQ2MQ==&mid=2247525904&idx=1&sn=cf9c8744b55293bd142c5ff5952ee86d&chksm=c3593bbaf42eb2ac7608f1d2c425b77e61e5f9da9bd42b0bc63253df785b51ee4bc3c84100eb&scene=21#wechat_redirect)，但又想知道 Scikit Optimize 是否适合？这时，你应当认真阅读完本文，**并且点个赞加个收藏～**

## SKOPT 入门

几乎所有实际情况，都需要搜索在交叉验证中能够让机器学习模型达到最佳性能的参数，这类参数可以使得模型达到最佳但泛化性。scikit-learn 中的一个标准方法是使用`sklearn.model_selection.GridSearchCV`类，它为每个参数尝试一组值，并简单地枚举参数值的所有组合。随着新参数的增加，这种搜索的复杂性呈指数增长。一种更具可扩展性的方法是使用`sklearn.model_selection.RandomizedSearchCV`，但是它没有利用搜索空间的结构。

Scikit-optimize 算是`sklearn.model_selection.GridSearchCV`的一个替代品，它利用贝叶斯优化，其中一个称为“surrogate”的预测模型用于对搜索空间进行建模，并用于尽快获得良好的参数值组合。

SKOPT 通过创建另一个模型来简化超参数优化，该模型通过更改其超参数试图最小化初始模型损失。

## 搜索空间

Scikit Optimize 的 API 真的特别好用。首先定义搜索空间：

```
SPACE = [
   skopt.space.Real(0.01, 0.5, name='learning_rate', prior='log-uniform'),
   skopt.space.Integer(1, 30, name='max_depth'),
   skopt.space.Integer(2, 100, name='num_leaves'),
   skopt.space.Integer(10, 1000, name='min_data_in_leaf'),
   skopt.space.Real(0.1, 1.0, name='feature_fraction', prior='uniform'),
   skopt.space.Real(0.1, 1.0, name='subsample', prior='uniform'),
  skopt.space.Categorical(categories = [True, False],name="bootstrap")]

```

搜索空间定义了我们想要在搜索中探索的超参数以及探索边界。大多数参数是整数、实数（浮点数）或分类。可以使用 skopt.space 类为每个参数定义搜索空间。即可以从三个选项中进行选择：

- `space.Real`\- 用于浮点数参数，从(a,b)范围通过均匀对数均匀采样
    
- `space.Integer`\- 用于整数参数，从(a,b)范围内均匀采样，
    
- `space.Categorical`\- 用于分类（文本）参数，将从选项列表中抽取一个值。例如，如果正在训练 lightGBM，则可以通过 \['gbdt','dart','goss'\] 选择参数值。
    

它不支持**嵌套搜索空间**，这就解释了某些超参数组合完全无效，但有时候真的很方便的情况。

## 超参数优化参数

```
HPO_PARAMS = { 
              'n_calls':100, 
              'n_random_starts':20, 
              'base_estimator':'ET', 
              'acq_func':'EI', 
             }

```

**HPO_PARAMS** 定义了用来寻找最佳参数的过程的一些基本属性。

- `n_calls`定义了要进行多少次参数迭代。
    
- `n_random_starts`定义了模型将进行的随机迭代次数，以便在开始寻找最佳位置之前对搜索空间进行更广泛的探索。在这种情况下，让模型在开始寻找最佳区域之前随机探索搜索空间进行 20 次迭代。
    
- `base_estimator`选择用于优化初始模型的超参数的模型，“ET”代表额外的应力回归器。
    
- `acq_func`定义了最小化的函数，**“EI”**指我们希望使用损失度量的减少作为改进目标。
    

## 模型最小化目标

**定义最小化的目标函数。**

```
@skopt.utils.use_named_args(SPACE)
def objective(**params):
    all_params = {**params, **STATIC_PARAMS}
    return (evaluator.evaluate_params(model, all_params))

```

模型使用目标函数来衡量每次迭代，在提高基础模型性能方面的效果。它将每次迭代中选择的超参数组合作为输入，并输出基本模型性能（隐藏在评估器类中）。

使用`@skopt.utils.used_named_args`包装器来转换目标函数，以便它接受列表参数（默认由优化器传递），同时保留特征名称。

## 评估器

设置评估器类的评估参数函数。创建了一个类来将与模型训练和评估相关的所有代码与模型隔离开来，以提高可读性。

上下滑动查看更多源码

```
def calculate_rmse(model, X, y):
    y_hat = model.predict(X)
    y_true = y
    rmse = np.sqrt(((y_true.values - y_hat)**2).mean())
return(rmse)
class Params_Evaluate():
def __init__(self, X_train, X_val, y_train, y_val):
        self.X_train = X_train
        self.X_val =  X_val
        self.y_train = y_train
        self.y_val = y_val
        self.n=0
def select_model(self, model):
        self.model = model
def evaluate_params(self,params):
        model =  self.model.set_params(**params)
        model.fit(self.X_train, self.y_train)
        rmse_train = calculate_rmse(model, self.X_train, self.y_train)
        rmse_val = calculate_rmse(model, self.X_val, self.y_val)
        print("Iteration {} with RMSE = {:.1f}/{:.1f} (validation/train) at {}"\
              .format(self.n, rmse_val, rmse_train, str(datetime.now().time())[:8]))
        self.n+=1
return(rmse_val)
```

使用训练和验证数据实例化评估器类，选择我们想要评估的模型，然后搜索最佳参数集以最小化验证集上的 RMSE。另外，在每次迭代后添加了一个打印语句，这样更容易跟踪进度。

```
evaluator = Params_Evaluate(X_train, X_val,
                            y_train, y_val) 
evaluator.select_model(
    model = RandomForestRegressor(n_jobs=4))

```

## 执行超参数优化

现在我们已经准备好使用最后一行代码开始搜索最佳超参数了：

```
results = skopt.forest_minimize(
        objective, SPACE, **HPO_PARAMS)

```

这里使用了`skopt.forest_minimize`函数，它使用了我们已经准备好的 3 个参数——objective、SPACE 和 HPO_PARAMS。

对于在`HPO_PARAMS`中定义的迭代次数，它在`SPACE`中选择一组参数，将它们提供给`objective`函数，目标函数使用`evaluator.evaluate_params`函数来检查这些参数在我们的模型中的执行情况。当我在`evaluate_params`函数中添加打印语句时，我们可以跟踪每次迭代之间的进度。

### 优化方法

**有四种优化算法可供选择：**

#### dummy_minimize

你可以对参数进行简单的随机搜索。这里没有什么特别之处，但是如果需要的话，在相同的API中使用这个选项进行比较是很有用的。

#### forest\_minimize 和 gbrt\_minimize

这两种方法以及下一节中的方法都是贝叶斯超参数优化(也称为基于顺序模型的优化SMBO)的例子。这种方法背后的思想是用随机森林、极度随机树或梯度增强树回归**估计**用户定义的**目标函数**。

在对目标函数的每一次超参数运行后，算法根据经验猜测哪一组超参数最有可能提高分数，应该在下一次运行中尝试。它是通过对许多点(超参数集)进行回归或预测，并根据所谓的获取函数选择最佳猜测点来完成的。

##### 有相当多的采集功能可供选择:

`EI和PI`：**负期望改进和负概率改进**。如果你选择其中一个，你也应该调整`xi`参数。基本上，当你的算法在寻找下一组超参数时，你可以决定你愿意在实际目标函数上尝试多大程度的预期改进。值越高，回归函数期望的改进(或改进的可能性)就越大。

`LCB`：**置信度下限**。在这种情况下，你需要仔细选择下一个点，限制下跌风险。可以决定在每次运行时要承担多大的风险。通过设置kappa参数越小，倾向于采用所有参数；通过设置kappa参数越大，倾向于采用搜索空间。

还有`EIPS和PIPS`选项，它们同时考虑了目标函数和执行时间产生的分数，但本文还没有尝试使用他们，感兴趣的读者可以尝试～

#### gp_minimize

该优化算法是近似的高斯过程而不是使用树回归。

从用户的角度来看，这种方法的附加价值在于，无需事先决定一个采集函数，而是可以让算法在每次迭代时选择EI、PI和LCB中的最佳函数。只需将采集函数设置为`gp_hedge`并进行试验。

需要考虑的另一件事是在每次迭代中使用的优化方法，即`sampling`或`lbfgs`。对于这两种方法，采集函数都是在搜索空间中随机选择的点数(n_points)上计算的。如果选择`sampling`，则选择值最小的点。如果选择`lbfgs`，算法将从最佳随机尝试点(`n_restarts_optimizer`) 中选取若干个点，并从每个点开始运行`lbfgs`优化。所以基本上，如果你不关心执行时间，`lbfgs`方法只是比`sampling`方法的一个改进。

使用官网strategy-comparison来比较以上几个优化方法。

使用benchmarks.branin函数作为昂贵函数的模型，此示例的目标是在尽可能少的迭代中找到这些最小值之一。一次迭代被定义为对benchmarks.branin函数的一次调用。

上下滑动查看更多源码

```
from skopt.benchmarks import branin as _branin
def branin(x, noise_level=0.):
return _branin(x) + noise_level * np.random.randn()
from functools import partial
from skopt import gp_minimize, forest_minimize, dummy_minimize
func = partial(branin, noise_level=2.0)
bounds = [(-5.0, 10.0), (0.0, 15.0)]
n_calls = 60
def run(minimizer, n_iter=5):
return [minimizer(func, bounds, n_calls=n_calls, random_state=n)
for n in range(n_iter)]
# Random search
dummy_res = run(dummy_minimize)
# Gaussian processes
gp_res = run(gp_minimize)
# Random forest
rf_res = run(partial(forest_minimize, base_estimator="RF"))
# Extra trees
et_res = run(partial(forest_minimize, base_estimator="ET"))
from skopt.plots import plot_convergence
plot = plot_convergence(("dummy_minimize", dummy_res),
                        ("gp_minimize", gp_res),
                        ("forest_minimize('rf')", rf_res),
                        ("forest_minimize('et)", et_res),
                        true_minimum=0.397887, yscale="log")
plot.legend(loc="best", prop={'size': 6}, numpoints=1)
```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

该图显示了找到的最小值（y 轴）作为迄今为止执行的迭代次数（x 轴）的函数。红色虚线表示**benchmarks.branin函数**最小值的真实值。

在前十次迭代中，所有方法的表现都一样好，因为它们都是在第一次拟合各自的模型之前创建十个随机样本开始的。在第 10 次迭代之后，下一个评估点**benchmarks.branin**由优化模型引导，也就是差异开始出现的地方。

## Callbacks回调

我非常喜欢有一个简单的选项来传递回调。例如，我可以通过简单地添加3行代码来监控训练。

```
# callback handler
def monitor(optim_result):
    score = searchcv.best_score_
    print("best score: %s" % score)
    if score >= 0.98:
        print('Interrupting!')
        return True
results = skopt.forest_minimize(
                    objective, SPACE, 
                    callback=[monitor], 
                    **HPO_PARAMS)

```

可以选择在每次迭代中提前停止或保存结果。

## 可视化评估结果

可以从评估收敛结果开始，看看我们的模型在每次迭代中的最佳性能如何提高。

可以使用 SKOPT 来可视化我超参数搜索，skopt中有三个绘图实用程序。不得不说可视化选项真的非常棒！

### plot_convergence

它通过在每次迭代中显示最好的结果来可视化优化的进展。

```
import skopt.plots
skopt.plots.plot_convergence(results)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

它最酷的地方在于，可以通过简单地传递结果对象列表或**(name, results)**元组列表来比较许多策略的进展。

```
results = [('random_results', random_results),
           ('forest_results', forest_results),
           ('gbrt_results', gbrt_results),
           ('gp_results', gp_results)]
skopt.plots.plot_convergence(*results)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### plot_evaluations

这个绘图可以让你看到搜索的发展过程。对于每个超参数，可以看到搜索值的直方图。对于每一对超参数，采样值的散点图用颜色表示，从蓝色到黄色。

例如，对于forest_minimize策略，可以清楚地看到它收敛于它更多地搜索的空间的某些部分。而随机搜索策略并不能看到这样的演变。

```
skopt.plots.plot_evaluations(results)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### plot_objective

你可以直观地了解与超参数相关的分数敏感性。可以决定空间的哪些部分可能需要更细粒度的搜索，哪些超参数几乎不影响分数，并且可能从搜索中删除。

```
skopt.plots.plot_objective(results)

```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## 输出最佳参数

我们可以通过调用`results.x`来获得最好的参数，但输出结果列表中没有 param_name ，所以这里我创建了一个辅助函数来将其转换为更易于阅读的字典。

```
def to_named_params(results, search_space):
    params = results.x
    param_dict = {}
    params_list  =[(dimension.name, param) for dimension, param in zip(search_space, params)]
    for item in params_list:
        param_dict[item[0]] = item[1]
    return(param_dict)
  
best_params = to_named_params(results, search_space) 
best_params

```

## 速度和并行化

每个优化函数都带有`n_jobs`参数，该参数被传递给`base_estimator`。这样的话，即使优化运行是按顺序进行的，我们也可以通过利用更多资源来加速每次运行。

## 保存与重启

有`skopt.dump`和`skopt.load`函数用于保存和加载结果对象。

```
results = skopt.forest_minimize(objective, SPACE, **HPO_PARAMS)
skopt.dump(results, 'artifacts/results.pkl')
old_results = skopt.load('artifacts/results.pkl')

```

可以通过`x0`和`y0`参数从保存的结果中重新启动训练。例如:

```
results = skopt.forest_minimize(
                 objective, SPACE,
                 x0=old_results.x_iters,
                 y0=old_results.func_vals,
                 **HPO_PARAMS)
```

总之，一方面有很多选项可以调优超参数，可以使用回调来控制训练。另一方面，你只能在平面空间中搜索，需要自己处理那些不可用的参数组合。

## 文档

它包含大量示例，所有函数和方法的文档字符串，并且只需要花了几分钟的时间就可以入门。完全可以自己去scikit-optimize官方文档网页看看。

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

## XGBoost调参实例

上下滑动查看更多源码

```
# 评估器类中选择新模型
evaluator.select_model(model = XGBRegressor())
# 重新定义搜索空间
search_space_xgb= [ 
         skopt.space.Integer(4, 5, name='max_depth'), 
         skopt.space.Real(0.0, 1.0, name='eta'), 
         skopt.space.Real(0.0, 1.0, name=' subsample'), 
         skopt.space.Categorical(categories = ["gbtree", "dart"],name="booster") 
         ]
# 重组目标函数
# 以便包装器使用更新后的 search_space 名称
```python
@skopt.utils.use_named_args(search_space_xgb)
def objective(**params):
return  evaluator.evaluate_params(params)
# 对 XGBoost 模型进行超参数优化
results=skopt.forest_minimize(objective, search_space_xgb,**HPO_params)
# 其他操作
best_params_gxb = to_named_params(results_xgb, search_space_xgb)
best_model = model_xgb.set_params(**best_params_gxb )
best_model.score(X_train, y_train)
best_model.score(X_val, y_val)
skopt.plots.plot_convergence(results_xgb)
skopt.plots.plot_objective(results_xgb)
```

## 写在最后

SKOPT 是一个很棒的的超参数优化工具，它结合了易用性和出色的可视化来分析结果。

该库也非常通用，因为我们可以随意设置目标函数，我们可以使用它来评估任何模型和任何超参数集。

最后，希望你看了近期的超参数优化系列文章，尽量不必在这些繁琐的网格搜索上浪费时间。这里可以尝试 SKOPT，这样你就可以在很短的时间内，利用另一个模型为你的目标模型找到最佳超参数。**点个赞哇！**

参考资料

\[1\]

scikit-optimize官方文档网: *https://scikit-optimize.github.io/stable/*

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

##### 🏴‍☠️宝藏级🏴‍☠️ 原创公众号『**数据STUDIO**』内容超级硬核。公众号以Python为核心语言，垂直于数据科学领域，包括可戳👉[**Python**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=1974978822768771072&scene=173&from_msgid=2247519294&from_itemidx=1&count=3&nolastread=1#wechat_redirect)**｜**[**MySQL**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=2023684574089658370&scene=173&from_msgid=2247519619&from_itemidx=2&count=3&nolastread=1#wechat_redirect)**｜**[**数据分析**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=1974978820940054530&scene=173&from_msgid=2247518366&from_itemidx=1&count=3&nolastread=1#wechat_redirect)**｜**[**数据可视化**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=1974991176839544834&scene=173&from_msgid=2247519244&from_itemidx=1&count=3&nolastread=1#wechat_redirect)**｜**[**机器学习与数据挖掘**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=1963494160565354497&scene=173&from_msgid=2247512171&from_itemidx=1&count=3&nolastread=1#wechat_redirect)**｜**[**爬虫**](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzk0OTI1OTQ2MQ==&action=getalbum&album_id=2318258648965644288&scene=173&from_msgid=2247518366&from_itemidx=1&count=3&nolastread=1#wechat_redirect) 等，从入门到进阶！

长按👇关注\- 数据STUDIO -设为星标，干货速递

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

People who liked this content also liked

Contextual Transformer端到端语音识别文本定制技术，可显著提升热词召回及整体识别率

...

语音之家

不看的原因

- 内容质量低
- 不看此公众号

CNS级宝藏公众号：R语言从入门到大师！

...

R语言数据分析指南

不看的原因

- 内容质量低
- 不看此公众号

机器学习十大热门算法

...

算法进阶

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___46d1f516622c438e8.bmp"/>

Scan to Follow