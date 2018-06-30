# Meeting Recordings 会议记录

## Metting 0 - Inception

> Time：2018-03-29 22:00:00~23:00:00

### 议题

> 中山大学第零饭堂扫码点餐系统

### 会议目标

> 定义产品范围、愿景和核心业务。

### Task1. 介绍产品调查结果

### Task2. 产品讨论

+ 确定产品名称：SYSU Canteen 0（中大零饭）

+ 候选业务范围：
  + [基本业务](https://github.com/dtosaad/documents/blob/master/product_requirements.md#基本业务)
  + [创新业务](https://github.com/dtosaad/documents/blob/master/product_requirements.md#创新业务)
  + [辅助业务](https://github.com/dtosaad/documents/blob/master/product_requirements.md#辅助业务)

+ 创新点凝练
  + 创新点 1：[扫码点餐](https://github.com/dtosaad/documents/blob/master/product_requirements.md#主要业务)
  + 创新点 2：[查看桌位空闲状态](https://github.com/dtosaad/documents/blob/master/product_requirements.md#创新业务)
  + 创新点 3：[协同点餐](https://github.com/dtosaad/documents/blob/master/product_requirements.md#创新业务)

### Task3. 定义产品

> 参考：[产品需求](https://github.com/dtosaad/documents/blob/master/product_requirements.md)

### Task5. 分析涉及的相关技术与潜在风险

+ Socket 双向通讯技术

协同点餐时，应该通过客户端与服务端的 Socket 连接来实现实时通讯，计时反应多个用户的点餐情况。

### Task6. 项目经理总结陈词

> “革命尚未成功，同志任需努力！”

## Metting 1 - Iteration 1

> 目标：扫码点餐

1. 产品经理完成扫码点餐的原型图（[Modao工具 最初原型](https://modao.cc/app/YiH5dTdxFF3JzQAkRsSjOWMHPRmoodZ)）
2. UI 设计师根据原型图设计页面细节
3. 前端工程师根据原型图
4. 后端工程师完成数据库表的设计、以及提供扫码点餐过程中设计的 API

## Metting 2 - Iteration 2

> 目标：协同点餐

协同点餐细节：

+ 一个桌子的两个或多个用户同时扫码点餐
+ 在购物车可以看到目前这个桌子所有的点餐信息
+ 点击完成点餐的时候：
  + 如果不是最后一个，则直接操作完成
  + 如果是最后一个点完餐的，则要负责确认订单 + 付款
+ 协同点餐用的抵用券来自于最后那个确认订单的用户，且最多只能用 1 张

另外，还有以下任务：

1. 完成建模练习：每个人都要选一个 app 进行建模练习
2. 解决 issue 里面提到的问题

## Metting 3 - Iteration 3

> 目标：查看桌位信息

细节：

+ 用户可以在"桌位"一栏看到餐厅内所有桌位的占用情况（如果某张桌位有人，则对应地会显示下单人的头像）
+ 用户看到空桌位的时候，可以点击之进行预订，桌位保留时间为 15 分钟，如果 15 分钟后客人未扫码点餐，则视为放弃

另外：

1. 每个人完成自己的技术报告
2. 前后端都写一份框架技术报告，最后合并一下
3. 完成『补充需求』：
  + 细化阶段补充明确的功能性需求
  + 补充文档的可靠性、可支持性、约束和接口模块
  + 补充文档的法律问题、所关注领域内的信息

![](/Users/minxinzhong/Documents/GitHub/documents/assets/images/tower.jpeg)

## Metting 4 - Iteration 4

> 目标：用餐反馈 & 菜品推荐 & 抵用券 & 外卖

### 1. 菜品反馈

用户在用餐之后，可以对每个菜品进行打分（评星级，满分五星）。这个反馈的信息汇总之后会在菜单界面显示出来。

### 2. 菜品推荐

后台会统计近期（最近 3/7 天）最受欢迎的五道菜并在小程序首页以轮播图的形式推荐给用户。

### 3. 抵用券

用户每次用完餐且满足以下额度的话，赠送抵用券（供下次使用），比如：

1. 消费满 100，送 10 元抵用券
2. 消费满 200，送 20 元抵用券
3. 消费满 300，送 30 元抵用券

消费满 k 百，则赠送 k 十元的优惠券（不满 100 的话无优惠券）。

### 4. 外卖

用户在确认订单的过程中，可以选择订单为外卖类型，并且需要提供额外的配送信息：

1. 姓名
2. 联系手机
3. 地址
