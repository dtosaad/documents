# API 文档

+ URL 前缀："https://${host}/weapp"
+ 每个 URL 后面都默认要加上 ?user_id= 的参数。

## 1. 扫码点餐

### 1.1. 用户登录

> POST /users/signin

服务器要保存 session_key 和 openid，然后根据 openid 返回 userid。

![](assets/images/wx_login.png)

```JSON
{
  "code": "xxx",
  "wechat_name": "xxx",
  "wechat_avatar": "xxx",
  "location": "xxx"
}
```

```JSON
{
  "userid": 1
}
```

### 1.2. 获取用户信息

> GET /users/:userid/contact

```JSON
{
  "wechat_name": "偷外卖死全家",
  "phone": 15521221390,
  "location": "中山大学东校区慎思园 6 号"
}
```

### 1.3. 获取所有菜品列表

> GET /dishes

```JSON
[
  {
    "dish_id": 1,
    "type": "焗饭",
    "image": "url",
    "dish_name": "蛋炒饭",
    "ordered_count": 999,
    "price": 10.00,
    "star_times": 100,
    "star_count": 450
  }
]
```

- dish_id(int)：菜品id
- type(string)：菜品所在分类
  - 分类具体有：铁板，披萨，焗饭，小吃，饮料。
- image(string)：菜品的图片url吧？没想好
- dish_name(string)：菜品名字
- ordered_count(int)：菜品的存量
- price(float)：菜品的价格
- star_times：用户评星次数
- star_count：累计星数

### 1.4. 获取用户吃过的菜品列表

> GET /dishes?userid=

+ `userid` 用户 id

```JSON
[
  {
    "dish_id": 1,
    "name": "蛋炒饭",
    "user_ordered_count": 2
  }
]
```

+ `user_ordered_count` 用户点该道菜点了几次

### 1.5. 获取每日推荐的图片链接

> GET /images/recommendation?number=

+ `number` 要获取的图片数目

```JSON
[
  "xxx"
]
```

### 1.6. 新建订单

> POST /orders

```JSON
{
  "table_id": 1,
  "dishes": [
    {
      "dish_id": 1,
      "dish_name": "蛋炒饭",
      "price": 10.00,
      "amount": 2
    }
  ],
  "people_count": 2,
  "dinning_choice": 1,
  "note": "不要辣",
  "takeout_info": {
    "name": "偷外卖死全家",
    "phone": 15521221390,
    "location": "中山大学东校区慎思园 6 号"
  },
  "discount_id": 1
}
```

+ `eating_mode` 用餐方式，`0` 代表堂食，`1` 代表外带，`2` 代表外卖
+ `takeout_info` 外卖信息，如果用餐方式不是外卖，这一项为 `null`
+ `discount_id` 如果没有优惠券则为 `null`

```JSON
{
  "order_id": 1
}
```

### 1.7 加菜

> PUT /orders/:order_id

```JSON
[
  {
    "dish_id": 1,
    "amount": 2,
    "price": 10.00
  }
]
```

表示 1 号菜加两份，总共需要加钱 20.00.

### 1.8 付款（非协同模式下）

> POST /orders/:order_id/pay

```JSON
{
  "table_id": 1
}
```

## 2. 协同点餐

table 表需要增加一个 orderers_count, orderers_total（整数）以及 user_avatar（字符串）的字段，表示正在该桌子上点餐的人数，用于协同点餐的人数判断。

### 2.1. 获取桌子信息

> GET /tables/:table_id

用户扫码进入小程序时，会自动出发此 API，服务端从数据库查询 table 信息，如果发现 user_id 为 null 时，要改为当前用户的 user_id，而且要设 orderers_count 为 1。

```JSON
{
  "table_id": 1,
  "number": "A1",
  "user_id": 1,
  "order_id": 1,
  "orderers_count": 2,
  "orderers_total": 3,
  "user_avatar": "xxx",
  "status": 1
}
```

`status`：0 代表空闲，1 代表有人，2 代表被预订

### 2.1. 确认参与协同点餐

> POST /tables/:table_id/together

用户进入菜单页的时候，如果检测到已经有人在点餐，则前端提示是否进入协同点餐模式，用户选择是则触发此 API。对应 table 的 orderers_count 要加 1。

### 2.2. 上传当前已点的菜品

> POST /tables/:table_id/dishes

```JSON
[]
```

返回最新的协同点餐信息：

```JSON
[]
```

### 2.3. 协同模式下确认点餐完毕

> POST /tables/:table_id/commit

前端记得在 commit 之前要上传下当前已点的菜品。

### 2.4 付款（协同模式下）

> POST /orders/:order_id/pay/together

```JSON
{
  "table_id": 1
}
```

## 3. 显示桌位

### 3.1. 获取当前所有桌位信息

> GET /tables

```JSON
[
  {
    "table_id": 1,
    "number": "A1",
    "user_id": 1,
    "user_avatar": "这里是用户头像链接"
  }
]
```

### 3.2. 坐下/预定桌位

> POST /tables/:table_id

```
{}
```

### 3.2. 离开/取消预订桌位

> DELETE /tables/:table_id

## 4. 其它

### 4.1. 菜品反馈

> POST /dishes/:dish_id/review

```JSON
{
  "star": 4.5
}
```

### 4.2. 菜品推荐

参考上面 1.5 的 API 即可。

### 4.3. 抵用券

#### 4.3.1 获取用户当前的抵用券

> GET /discounts

```JSON
[
  {
    "discount_id": 1,
    "discount": 50
  }
]
```

#### 4.3.2 使用抵用券

参考上面 1.6 的 API，需要增加一个 discount_id 的参数（数据库需要增加一张 user_discount 的表来存用户的抵用券），来表明用哪一张抵用券。1.6 那里的代码逻辑需要判断是否有 discount_id，有则要去数据库取出来减去相应金额并删除之。

### 4.4. 外卖

上面 1.2 有获取用户配送信息的 API。
