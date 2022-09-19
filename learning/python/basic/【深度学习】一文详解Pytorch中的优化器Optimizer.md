# 【深度学习】一文详解Pytorch中的优化器Optimizer

<a id="profileBt"></a><a id="js_name"></a>机器学习初学者 *2022-04-30 12:00* *Posted on <a id="js_ip_wording"></a>浙江*

The following article is from AI算法小喵 Author 小喵

<a id="copyright_info"></a>[![](../../../_resources/0_6c31ebd89d414e8dbc5cd41bf1b9d237.jpg)<br>**AI算法小喵** .<br>专注于分享深度学习、机器学习、NLP等方向的干货知识，一起学习，共同进步。](#)

1\. 前言

**优化器**主要用在模型训练阶段，用于更新模型中可学习的参数。torch.optim<sup>\[1\]</sup>提供了多种优化器**接口**，比如Adam、RAdam、SGD、ASGD、LBFGS等，`Optimizer`是所有这些优化器的**父类**。

## 2\. Optimizer行为解析

### 2.1 公共方法

`Optimizer`是所有优化器的父类，它主要具有以下几类公共方法：

| 方法名 | 官方注解 | 说明  |
| --- | --- | --- |
| `add_param_group` | Add a param group to the Optimizer's param_groups. | 给优化器添加模型中可学习的参数组。 |
| `step` | Performs a single optimization step (parameter update) | 进行一次参数更新。 |
| `zero_grad` | Sets the gradients of all optimized torch.Tensor to zero. | 将上次记录的梯度信息置零，避免梯度累加。 |
| `state_dict` | Returns the state of the optimizer as a dict. | 以字典形式返回优化器的状态信息。 |
| `load_state_dict` | Loads the optimizer state. | 加载以字典形式存储的优化器状态信息。 |

### 2.2 行为解析

以下将结合源码与示例代码解析`Optimizer`各种方法的具体行为。为了方便理解，小喵在相关源码中都添加了必要的注释。

#### 2.2.1 \_\_init\_\_\_初始化

下面是`Optimizer`的初始化函数源码:

```
def __init__(self, params, defaults):
    """
    :param params: 待优化参数params，可以有两种格式，分别对应全局参数、参数组
    :defaults: （全局）默认超参数字典，这里的超参数主要指优化器参数，如学习率等。
    """
    torch._C._log_api_usage_once("python.optimizer")
    self.defaults = defaults
    if isinstance(params, torch.Tensor):
        raise TypeError("params argument given to the optimizer should be "
                        "an iterable of Tensors or dicts, but got " +
                        torch.typename(params))
    self.state = defaultdict(dict)
    self.param_groups = []
    param_groups = list(params)
    if len(param_groups) == 0:
        raise ValueError("optimizer got an empty parameter list")
    # 如果是全局参数，则转换为字典格式，并放入列表中
    if not isinstance(param_groups[0], dict):
        param_groups = [{'params': param_groups}]
    for param_group in param_groups:
        self.add_param_group(param_group)

```

初始化函数用于初始化优化器，需传入两个参数：

- `params`：模型的可学习参数。
    
- `defaults`：(全局)默认优化器超参，如学习率`lr`等。
    

`params`允许两种传入格式，其一是网络全局参数，即网络的所有可学习参数，它们共用一套优化器参数：

```
# 全局参数示例：
# 假设net为我们所创建的网络模型
# 直接调用net的`.parameters()`方法即可获得网络全局参数
# 网络全局参数使用相同的学习率lr=0.001进行参数更新
params = net.parameters()
optimizer = torch.optim.Adam(params, lr=0.001)

```

其二是参数组，每组参数可以指定自己的优化器参数，即每组参数可使用不同的优化策略：

```
# 参数组示例：
# 假设net为我们所创建的网络模型
# 按网络参数的参数名是否含有bert为条件，将net的所有可学习参数分为两组
# 第二组参数使用学习率lr=2e-5
params = [
    {
        "params":[p for n, p in net.named_parameters() if "bert" not in n ]
    },
    {
        "params":[p for n, p in net.named_parameters() if "bert" in n ],
        'lr': 2e-5
    }
]
optimizer = torch.optim.Adam(params, lr=0.001)

```

> 补充知识：named_parameters()返回关于网络层参数名字和参数，parameters()仅返回网络层参数。

#### 2.2.2 add\_param\_group参数组配置

在初始化函数中，调用了`add_param_group`方法往`Optimizer.params_groups`中添加分组参数，`add_param_group`的源码如下：

```
def add_param_group(self, param_group):
    r"""Add a param group to the :class:`Optimizer` s `param_groups`.
    This can be useful when fine tuning a pre-trained network as frozen layers can be made
    trainable and added to the :class:`Optimizer` as training progresses.
    Arguments:
        param_group (dict): Specifies what Tensors should be optimized along with group
        specific optimization options.
    """
    # 每组参数必须是字典格式
    assert isinstance(param_group, dict), "param group must be a dict"
    params = param_group['params']
    if isinstance(params, torch.Tensor):
        param_group['params'] = [params]
    elif isinstance(params, set):
        raise TypeError('optimizer parameters need to be organized in ordered collections, but '
                        'the ordering of tensors in sets will change between runs. Please use a list instead.')
    else:
        param_group['params'] = list(params)
    # 待优化参数必须为张量
    for param in param_group['params']:
        if not isinstance(param, torch.Tensor):
            raise TypeError("optimizer can only optimize Tensors, "
                            "but one of the params is " + torch.typename(param))
        # 待优化参数必须为叶子结点
        if not param.is_leaf:
            raise ValueError("can't optimize a non-leaf Tensor")
    for name, default in self.defaults.items():
        if default is required and name not in param_group:
            raise ValueError("parameter group didn't specify a value of required optimization parameter " +
                             name)
        # 对该组参数没有指定的超参数，则设置为（全局）默认优化器参数中相应的值。
        else:
            param_group.setdefault(name, default)
    # 该组参数不允许出现在其它参数组中，即参数组之间交集为空。
    param_set = set()
    for group in self.param_groups:
        param_set.update(set(group['params']))
    if not param_set.isdisjoint(set(param_group['params'])):
        raise ValueError("some parameters appear in more than one parameter group")
    self.param_groups.append(param_group)

```

一方面，针对每一组参数，`add_param_group`会判断它们是否违背了相关条件，比如：非张量、非叶子结点、出现在其他参数组中。

另一方面，`add_param_group`会将没有单独指定优化器超参的参数组的优化器参数设定（全局）默认优化器超参值。比如在之前的例子中，第一组参数的学习率`lr=0.001`，第二组参数的学习率`lr=2e-5`。

```
# 参数组形式：
params = [
    {
        "params":[p for n, p in net.named_parameters() if "bert" not in n ]
    },
    {
        "params":[p for n, p in net.named_parameters() if "bert" in n ],
        'lr': 2e-5
    }
]
optimizer = torch.optim.Adam(params, lr=0.001)

```

#### 2.2.3 step更新参数

`step`方法作用是执行一次参数的更新。 `Optimizer` 定义了 `step` 方法接口，所有继承自它的子类都需要对`step`进行实现。

```
# Optimizer的step
def step(self, closure):
      r"""Performs a single optimization step (parameter update).
      Arguments:
          closure (callable): A closure that reevaluates the model and
              returns the loss. Optional for most optimizers.
      """
      raise NotImplementedError

```

下面，我们以`SGD`优化器为例，来看`step`方法的具体行为，相关源码如下：

```
# SGD的step
def step(self, closure=None):
    """Performs a single optimization step.
    Arguments:
        closure (callable, optional): A closure that reevaluates the model
            and returns the loss.
    """
    loss = None
    if closure is not None:
        loss = closure()
    # 针对每个参数组
    for group in self.param_groups:
        # 取出该组参数更新时所需要的优化器超参数
        weight_decay = group['weight_decay']
        momentum = group['momentum']
        dampening = group['dampening']
        nesterov = group['nesterov']
        # 取出该组参数中待更新的参数
        for p in group['params']:
            if p.grad is None:
                continue
            # 取出参数的梯度值
            d_p = p.grad.data
            # 加入正则项，避免过拟合
            if weight_decay != 0:
                d_p.add_(weight_decay, p.data)
            # 计算动量，加速梯度下降
            if momentum != 0:
                param_state = self.state[p]
                if 'momentum_buffer' not in param_state:
                    buf = param_state['momentum_buffer'] = torch.clone(d_p).detach()
                else:
                    buf = param_state['momentum_buffer']
                    buf.mul_(momentum).add_(1 - dampening, d_p)
                if nesterov:
                    d_p = d_p.add(momentum, buf)
                else:
                    d_p = buf
            # 更新参数 p.data=p.data-lr*d_p
            p.data.add_(-group['lr'], d_p)
    return loss

```

从源码来看，针对每组参数，参数更新过程如下：

- 首先取出参数的梯度值；
    
- 然后为避免过拟合，利用优化器超参中的`weight_decay`为梯度添加了正则项；
    
- 接着为加速，利用`momentum、dampening、nesterov`计算动量，更新梯度；
    
- 最后，利用梯度更新参数值。
    

> 这里我们暂不展开讲解闭包函数closure知识点。

#### 2.2.4 zero_grad清零梯度

`zero_grad`方法一般用在反向传播之前，作用是将上次反向传播时记录的梯度值清零（在[Pytorch张量属性与梯度计算那些事儿（下）](http://mp.weixin.qq.com/s?__biz=Mzg4OTY3ODQ1NQ==&mid=2247484766&idx=1&sn=e00d898dafe012047d33cbec36e19a1d&chksm=cfe97a46f89ef3503e70efbea6a88e982f7f3d5c275a07fbaaad336ccdac17175b6f2fc989e6&scene=21#wechat_redirect)一文中已提到过，如果不清零梯度，梯度会叠加）。

下面是`Optimizer`的`zero_grad`方法的源码：

```
def zero_grad(self):
    r"""Clears the gradients of all optimized :class:`torch.Tensor` s."""
    for group in self.param_groups:
        for p in group['params']:
            if p.grad is not None:
                # 截断反向传播的梯度流
                p.grad.detach_()
                # 清零已存储的梯度值
                p.grad.zero_()

```

#### 2.2.5 state_dict保存参数与状态

`state_dict`方法返回优化器管理的参数与状态信息，下面是`Optimizer`的`state_dict`方法的源码：

```
def state_dict(self):
    r"""Returns the state of the optimizer as a :class:`dict`.
    It contains two entries:
    * state - a dict holding current optimization state. Its content
        differs between optimizer classes.
    * param_groups - a dict containing all parameter groups
    """
    # Save ids instead of Tensors
    def pack_group(group):
        packed = {k: v for k, v in group.items() if k != 'params'}
        packed['params'] = [id(p) for p in group['params']]
        return packed
    # 保存每组参数中相关参数的地址及优化器超参
    param_groups = [pack_group(g) for g in self.param_groups]
    # Remap state to use ids as keys
    # 将state中所有参数张量替换为张量对象地址保存
    packed_state = {(id(k) if isinstance(k, torch.Tensor) else k): v
                    for k, v in self.state.items()}
    return {
        'state': packed_state,
        'param_groups': param_groups,
    }

```

从源码可知，当我们调用`state_dict`方法时会获得一个字典形式的返回值，它包含两项内容：

- `state` (dict) ：保存了优化器状态信息，键为网络模型参数的地址，值为在更新网络参数的过程中计算出的与该参数相关的缓存变量信息，如`momentum_buffer`。
    
- `param_groups`(list) ：列表中每一项是一个字典，存储了相应的一组参数的参数地址及该组参数相关的优化器参数。
    

下面我们沿用之前在[Pytorch张量属性与梯度计算那些事儿（下）](http://mp.weixin.qq.com/s?__biz=Mzg4OTY3ODQ1NQ==&mid=2247484766&idx=1&sn=e00d898dafe012047d33cbec36e19a1d&chksm=cfe97a46f89ef3503e70efbea6a88e982f7f3d5c275a07fbaaad336ccdac17175b6f2fc989e6&scene=21#wechat_redirect)中给出的示例来让大家对`state_dict`方法对返回值有更直接和清晰的认识：

```
#!/usr/local/bin/python3
# -*-coding:utf-8 -*-
import torch
import torch.nn as nn
import json
class DemoNet(nn.Module):
    def __init__(self):
        super(DemoNet, self).__init__()
        self.fc = nn.Linear(3, 3)
        self.fc1 = nn.Linear(3, 1)
    def forward(self, x):
        x1 = self.fc(x)
        y = self.fc1(x1)
        z = torch.sigmoid(y)
        return z
if __name__ == "__main__":
    # 模拟输入和标签
    x = torch.randn((100, 3))
    # 每个样本标签为1或0
    y = torch.empty(100).random_(2)
    # 网络初始化
    demoNet = DemoNet()
    # 定义损失
    loss_func = nn.BCELoss()
    # 定义优化器
    optimizer=optim.SGD(demoNet.parameters(), lr=1e-3)
    # 清空梯度
    optimizer.zero_grad()
    # 前向传播，进行预测
    y_hat = demoNet(x)
    # 反向传播，计算梯度
    y_hat = torch.squeeze(y_hat, dim=-1)
    loss = loss_func(y_hat, y)
    loss.backward()
    optimizer.step()
    print(json.dumps(optimizer.state_dict(),indent=4))

```

运行之后的结果如下，state中是空的，`param_groups`则保存了参数组中参数的地址及优化器参数：

```
{
    "state": {},
    "param_groups": [
        {
            "lr": 0.001,
            "momentum": 0,
            "dampening": 0,
            "weight_decay": 0,
            "nesterov": false,
            "params": [
                4821053656,
                4821053728,
                4821053800,
                4821053872
            ]
        }
    ]
} 

```

更改下优化器与打印语句（因为张量是不能被序列化为json对象，会报`Object of type Tensor is not JSON serializable`错误）

```
if __name__ == "__main__":
    # 模拟输入和标签
    x = torch.randn((100, 3))
    # 每个样本标签为1或0
    y = torch.empty(100).random_(2)
    # 网络初始化
    demoNet = DemoNet()
    # 定义损失
    loss_func = nn.BCELoss()
    # 定义优化器
    # optimizer=optim.SGD(demoNet.parameters(), lr=1e-3)
    optimizer = torch.optim.SGD(demoNet.parameters(),
                                momentum=0.8,
                                weight_decay=1e-5,lr=1e-3)
    # 清空梯度
    optimizer.zero_grad()
    # 前向传播，进行预测
    y_hat = demoNet(x)
    # 反向传播，计算梯度
    y_hat = torch.squeeze(y_hat, dim=-1)
    loss = loss_func(y_hat, y)
    loss.backward()
    optimizer.step()
    # print(json.dumps(optimizer.state_dict(),indent=4))
    print(optimizer.state_dict())

```

再次运行后，`state`中保存了参数组中参数的地址及相应的缓存变量momentum_buffer的信息。

```
{'state': 
    {
    4727062392: {'momentum_buffer': tensor([[ 0.0028,  0.0005,  0.0047],
        [ 0.0145,  0.0027,  0.0243],
        [-0.0184, -0.0034, -0.0308]])}, 
    4727062464: {'momentum_buffer': tensor([ 0.0016,  0.0083, -0.0106])}, 
    4727083080: {'momentum_buffer': tensor([[-0.0516, -0.0293,  0.0673]])}, 
    4727083152: {'momentum_buffer': tensor([-0.0328])}
    }, 
'param_groups': [
    {
        'lr': 0.001, 
        'momentum': 0.8, 
        'dampening': 0, 
        'weight_decay': 1e-05, 
        'nesterov': False, 
        'params': [4727062392, 4727062464, 4727083080, 4727083152]
    }
  ]
}

```

#### 2.2.6 load\_state\_dict加载参数与状态

相应地，`load_state_dict`则是用于加载所保存的优化器管理的参数与状态信息，其源码如下：

```
def load_state_dict(self, state_dict):
    r"""Loads the optimizer state.
    Arguments:
        state_dict (dict): optimizer state. Should be an object returned
            from a call to :meth:`state_dict`.
    """
    # deepcopy, to be consistent with module API
    state_dict = deepcopy(state_dict)
    # Validate the state_dict
    groups = self.param_groups
    saved_groups = state_dict['param_groups']
    # 判断当前优化器中的待优化参数与加载的待优化参数在size上是否一致
    if len(groups) != len(saved_groups):
        raise ValueError("loaded state dict has a different number of "
                         "parameter groups")
    param_lens = (len(g['params']) for g in groups)
    saved_lens = (len(g['params']) for g in saved_groups)
    if any(p_len != s_len for p_len, s_len in zip(param_lens, saved_lens)):
        raise ValueError("loaded state dict contains a parameter group "
                         "that doesn't match the size of optimizer's group")
    # Update the state
    # 建立参数对象旧地址与参数对象的映射关系
    id_map = {old_id: p for old_id, p in
              zip(chain(*(g['params'] for g in saved_groups)),
                  chain(*(g['params'] for g in groups)))}
    def cast(param, value):
        r"""Make a deep copy of value, casting all tensors to device of param."""
        if isinstance(value, torch.Tensor):
            # Floating-point types are a bit special here. They are the only ones
            # that are assumed to always match the type of params.
            if param.is_floating_point():
                value = value.to(param.dtype)
            value = value.to(param.device)
            return value
        elif isinstance(value, dict):
            return {k: cast(param, v) for k, v in value.items()}
        elif isinstance(value, container_abcs.Iterable):
            return type(value)(cast(param, v) for v in value)
        else:
            return value
    # Copy state assigned to params (and cast tensors to appropriate types).
    # State that is not assigned to params is copied as is (needed for
    # backward compatibility).
    state = defaultdict(dict)
    for k, v in state_dict['state'].items():
        # 参数对象的旧地址
        if k in id_map:
            # 参数对象
            param = id_map[k]
            # 转换参数的数据类型与device
            state[param] = cast(param, v)
        else:
            state[k] = v
    # Update parameter groups, setting their 'params' value
    # 以保存好的参数值来更新模型参数
    def update_group(group, new_group):
        new_group['params'] = group['params']
        return new_group
    param_groups = [
        update_group(g, ng) for g, ng in zip(groups, saved_groups)]
    self.__setstate__({'state': state, 'param_groups': param_groups})

```

这里还是通过示例来让大家对`load_state_dict`方法有一个更直接的印象。

沿用刚才那个例子，在进行完一次反向传播和参数更新以后，将优化器管理的参数与状态信息保存起来：

```
# 保存优化器信息
if __name__ == "__main__":
    save_path="optimizer.pt"
    # 模拟输入和标签
    x = torch.randn((100, 3))
    # 每个样本标签为1或0
    y = torch.empty(100).random_(2)
    # 网络初始化
    demoNet = DemoNet()
    # 定义损失
    loss_func = nn.BCELoss()
    # 定义优化器
    optimizer = torch.optim.SGD(demoNet.parameters(),
                                momentum=0.8,
                                weight_decay=1e-5,lr=1e-3)
    # 清空梯度
    optimizer.zero_grad()
    # 前向传播，进行预测
    y_hat = demoNet(x)
    # 反向传播，计算梯度
    y_hat = torch.squeeze(y_hat, dim=-1)
    loss = loss_func(y_hat, y)
    loss.backward()
    optimizer.step()
    print(optimizer.state_dict())
    # 保存参数与状态信息
    torch.save(optimizer.state_dict(), save_path)

```

然后再修改代码重新运行，利用`load_state_dict`将其加载进来：

```
# 加载保存的优化器信息
if __name__ == "__main__":
    save_path="optimizer.pt"
    # 模拟输入和标签
    x = torch.randn((100, 3))
    # 每个样本标签为1或0
    y = torch.empty(100).random_(2)
    # 网络初始化
    demoNet = DemoNet()
    # 定义损失
    loss_func = nn.BCELoss()
    # 定义优化器
    optimizer = torch.optim.SGD(demoNet.parameters(),
                                momentum=0.8,
                                weight_decay=1e-5,lr=1e-3)
    # 将保存好的参数与状态信息加载到内存
    loaded_stated = torch.load(save_path)
    # 加载的结果
    print("loaded: \n", loaded_stated)
    # 将加载到内存的参数与状态信息加载到优化器
    optimizer.load_state_dict(loaded_stated)
    # 再次调用state_dict查看优化器参数与状态信息
    print("state_dict: \n",optimizer.state_dict())

```

这里，我们将保存时的参数与状态信息、加载到内存的参数与状态信息、从内存加载到优化器的参数与状态信息一并展示。

可以发现一个之前在 2.2.5 小节中已经出现但还没被提及的现象：参数的`id`会发生变化。

```
# 保存时打印结果
{'state': {
    4531957696: {'momentum_buffer': tensor([[ 0.0215,  0.0111, -0.0120],
        [-0.0610, -0.0314,  0.0342],
        [-0.0103, -0.0053,  0.0058]])}, 
    4531978312: {'momentum_buffer': tensor([ 0.0061, -0.0174, -0.0029])}, 
    4531978384: {'momentum_buffer': tensor([[-0.0171,  0.0468, -0.0662]])}, 
    4531978456: {'momentum_buffer': tensor([-0.0339])}}, 
'param_groups': [
    {
        'lr': 0.001, 
        'momentum': 0.8, 
        'dampening': 0, 
        'weight_decay': 1e-05, 
        'nesterov': False, 
        'params': [4531957696, 4531978312, 4531978384, 4531978456]
    }
  ]
}
# 加载到内存
loaded:
 {'state': {
        4531957696: {'momentum_buffer': tensor([[ 0.0215,  0.0111, -0.0120],
        [-0.0610, -0.0314,  0.0342],
        [-0.0103, -0.0053,  0.0058]])}, 
        4531978312: {'momentum_buffer': tensor([ 0.0061, -0.0174, -0.0029])}, 
        4531978384: {'momentum_buffer': tensor([[-0.0171,  0.0468, -0.0662]])}, 
        4531978456: {'momentum_buffer': tensor([-0.0339])}}, 
    'param_groups': [
        {
            'lr': 0.001, 
            'momentum': 0.8, 
            'dampening': 0, 
            'weight_decay': 1e-05, 
            'nesterov': False, 
            'params': [4531957696, 4531978312, 4531978384, 4531978456]
        }
    ]
}
# 加载到优化器
state_dict：
  {'state': {
        4683133000: {'momentum_buffer': tensor([[ 0.0215,  0.0111, -0.0120],
        [-0.0610, -0.0314,  0.0342],
        [-0.0103, -0.0053,  0.0058]])}, 
        4683133072: {'momentum_buffer': tensor([ 0.0061, -0.0174, -0.0029])}, 
        4683133144: {'momentum_buffer': tensor([[-0.0171,  0.0468, -0.0662]])}, 
        4683133216: {'momentum_buffer': tensor([-0.0339])}}, 
    'param_groups': [
        {
            'lr': 0.001, 
            'momentum': 0.8, 
            'dampening': 0, 
            'weight_decay': 1e-05, 
            'nesterov': False, 
            'params': [4683133000, 4683133072, 4683133144, 4683133216]
        }
    ]
}
```

id() 函数返回的是相应对象在其生命周期内位于内存中的地址。比如下面这个例子中，我们相当于两次运行了相同的代码，变量a的值是一样的，但是python为a分配的内存地址是不同的。

```
`➜  ~ python3
Python 3.7.3 (default, Mar 27 2019, 09:23:39)
[Clang 10.0.0 (clang-1000.11.45.5)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> a=1
>>> id(a)
4504543392
>>> exit()
➜  ~ python3
Python 3.7.3 (default, Mar 27 2019, 09:23:39)
[Clang 10.0.0 (clang-1000.11.45.5)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> a=1
>>> id(a)
4376338592
>>>
`
```

好了，我们回到之前保存和加载优化器的例子中来。

当我们调用`torch.load`将保存的参数与状态信息加载进内存的时候，参数的`id`是没有变化的；但，实际上当我们重新运行代码走到网络初始化这步时，网络的参数已经被分配了新的内存地址，即参数有新的`id`了。

然后，在使用 `load_state_dict` 将内存中加载好的优化器信息置入模型当中时，源码中`id_map`建立了参数对象旧地址与参数对象的映射关系，依据这个映射关系，优化器会更新它的`state。`所以，当我们调用`state_dict`后，所看到的参数`id`就是新的。

# 总结

本文结合源码和代码示例解析了**优化器**`Optimizer`的相关行为。在本文知识点的基础上，下一次，我们将一起学习与深度学习模型训练相关且需要掌握的两点内容：**分组优化**、**继续训练**。

### 参考资料

\[1\]

`torch.optim`: *https://pytorch.org/docs/stable/optim.html*

```


![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```


```


```


```


```


往期精彩回顾

- [适合初学者入门人工智能的路线及资料下载](http://mp.weixin.qq.com/s?__biz=MzIwODI2NDkxNQ==&mid=2247484737&idx=1&sn=27c52b4bc4ca98d3ab817344b84226cc&chksm=97048efda07307eb78d4f4ec0039a386a658404156b051af0cb715fafa8d2ae66cbe49343bf3&scene=21#wechat_redirect)
    
- [(图文+视频)机器学习入门系列下载](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzIwODI2NDkxNQ==&action=getalbum&album_id=2259163844755406853#wechat_redirect)
    
- [中国大学慕课《机器学习》（黄海广主讲）](http://mp.weixin.qq.com/s?__biz=MzIwODI2NDkxNQ==&mid=2247502323&idx=1&sn=598d7231681ce2f316503201dd615c86&chksm=9707424fa070cb59f861a47f9eb4218cc5d3b835be49e93e67bb086dcc4c3b555a3546ebe9c5&scene=21#wechat_redirect)
    
- [机器学习及深度学习笔记等资料打印](http://mp.weixin.qq.com/s?__biz=MzIwODI2NDkxNQ==&mid=2247488304&idx=1&sn=581944f63eab1822ca53b9a4eeedad79&chksm=9704988ca073119a38a534adbedd51ca0b5705cdd6a104fed74b265bb092485e97c91bb5b347&scene=21#wechat_redirect)
    
- [《统计学习方法》的代码复现专辑](https://mp.weixin.qq.com/mp/appmsgalbum?action=getalbum&album_id=1337257945842778113&__biz=MzIwODI2NDkxNQ==#wechat_redirect)
    
- [AI基础下载](http://mp.weixin.qq.com/s?__biz=MzIwODI2NDkxNQ==&mid=2247487602&idx=2&sn=478d9893e85d564282334654b3be7fda&chksm=97049bcea07312d8b74ac7e99e3a3dbea331315c1439888f4e16e912b18d3d8626f36e1fc0df&scene=21#wechat_redirect)
    
- 机器学习交流qq群955171419，加入微信群请扫码：
    




```

![Image](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)


```


```


```


```






```

People who liked this content also liked

分享10个超级实用事半功倍的Python自动化脚本

...

简说Python

不看的原因

- 内容质量低
- 不看此公众号

又一款python开发神器

...

基因学苑

不看的原因

- 内容质量低
- 不看此公众号

【Docker】命令使用大全

...

小白学视觉

不看的原因

- 内容质量低
- 不看此公众号

<img width="102" height="102" src="../../../_resources/qrcode_scene_10000004_size_102___91e1f2a4950a4d469.bmp"/>

Scan to Follow