# API 文档

## 1. 用户部分

1. 获取用户信息

> GET /users/:userid

```JSON
{
  "name": "偷外卖死全家",
  "phone": 15521221390,
  "location": "中山大学东校区慎思园 6 号"
}
```

## 2. 菜品部分

1. 获取所有菜品列表

> GET /dishes

```JSON
[
  {
    "dish_id": 1,
    "name": "蛋炒饭",
    "ordered_count": 999,
    "price": 10.00,
    "star_count": 420,
    "star_total": 500
  }
]
```

2. 获取用户吃过的菜品列表

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

3. 获取每日推荐的图片链接

> GET /images/recommendation?number=

+ `number` 要获取的图片数目

```JSON
[
  "xxx"
]
```

## 3. 订单部分

1. 新建订单

> POST /orders

```JSON
{
  "user_id": 1,
  "dishes": [
    {
      "dishid": 1,
      "name": "蛋炒饭",
      "price": 10.00,
      "amount": 2
    }
  ],
  "people_count": 2,
  "eating_mode": 1,
  "note": "不要辣",
  "takeout_info": {
    "name": "偷外卖死全家",
    "phone": 15521221390,
    "location": "中山大学东校区慎思园 6 号"
  }
}
```

+ `eating_mode` 用餐方式，`0` 代表堂食，`1` 代表外带，`2` 代表外卖
+ `takeout_info` 外卖信息，如果用餐方式不是外卖，这一项为 `null`
