---
title: 前端创建mock数据
abbrlink: 'mockFronted'
categories: 前端
tags: [前端, mock数据, easy-mock]
---
# 前端自己创建mock数据:easy-mock

一般情况下，测试接口的时候，都是后端mock数据，然后后端给前端一个swagger-ui（或者通过别的方式给你一个文档），然后前端进行api调用。但是有时候后端很忙，或者后端不想帮你mock数据，这时候就需要自己进行mock数据了，前端可以使用mock server，或者mockjs+rap，还可以使用easy-mock。在这里我们使用easy-mock的方法

在此之前

## Mock.js

easy-mock也是基于mock.js的语法规范进行设置的，所以在使用easy-mock之前，得先了解mock.js的使用。

### 数据模板定义规范 DTD

[所有样板示例](http://mockjs.com/examples.html)

[语法规范](https://github.com/nuysoft/Mock/wiki/Syntax-Specification)

数据模板中的每个属性由3部分构成： 属性名，生成规则，属性值：

```javascript
// 属性名   name
// 生成规则 rule
// 属性值   value
'name|rule': value
```

注意：

```
属性名 和 生成规则 之间用竖线 | 分隔。
生成规则 是可选的。
生成规则 有 7 种格式：
'name|min-max': value
'name|count': value
'name|min-max.dmin-dmax': value
'name|min-max.dcount': value
'name|count.dmin-dmax': value
'name|count.dcount': value
'name|+step': value
生成规则 的 含义 需要依赖 属性值的类型 才能确定。
属性值 中可以含有 @占位符。
属性值 还指定了最终值的初始值和类型。
```

举例：

```javascript
'name|min-max': string
通过重复 string 生成一个字符串，重复次数大于等于 min，小于等于 max。

模板:
 "string|1-10": "★"
 
结果:
{
  "string": "★★"
}
```

### 数据占位符定义规范

联合参考这两个文档:

[例子文档](http://mockjs.com/examples.html#DPD)

[例子解释文档](https://github.com/nuysoft/Mock/wiki/Basic)

数据占位符也是random，即不使用写死的数据，而是使用在一定范围内的mock自带的变化值。

占位符只是在属性值字符串中占个位置，并不出现在最终的属性值中：

```
@占位符
@占位符(参数 [, 参数])
```

举例：

```javascript
Mock.mock({
    name: {
        first: '@FIRST',
        middle: '@FIRST',
        last: '@LAST',
        full: '@first @middle @last'
    }
})
// =>
{
    "name": {
        "first": "Charles",
        "middle": "Brenda",
        "last": "Lopez",
        "full": "Charles Brenda Lopez"
    }
}
```

## easy-mock

创建账号-创建项目-设置BaseUrl-创建接口-在操作一栏下的编辑里进行代码编写（按照mock.js的方法）

### 常见使用方式-静态数据

静态数据最简单，随便写个json（符合自己的需求的），然后复制到编辑区：

```javascript
{
  name: '小明',
  age: 14,
  gender: true,
  height: 1.65,
  grade: null,
  'middle-school': '\"W3C\" Middle School',
  skills: ['JavaScript', 'Java', 'Python', 'Lisp']
}
```



### 常见使用方式-动态数据

改造一下JSON，编辑页面：

```
{
  "code": 0,
  "message": "success",
  "update_time": "@now",
  "data|20": [{
    "id": "@id",
    "wechat_id": "@word(5)",
    "phone": /^1[34578]\d{9}$/,
    "name": "@cname",
    "index|+1": 0
  }]
}
```

访问结果：

```
{
  "code": 0,
  "message": "success",
  "update_time": "2019-03-15 15:58:10",
  "data": [
    {
      "id": "130000198012071312",
      "wechat_id": "gbjrh",
      "phone": "15538214534",
      "name": "段超",
      "index": 0
    },
    {
      "id": "120000201604272428",
      "wechat_id": "ycskj",
      "phone": "17531635685",
      "name": "龙明",
      "index": 1
    },
    {
      "id": "360000198712315587",
      "wechat_id": "yayql",
      "phone": "15741728621",
      "name": "范秀英",
      "index": 2
    },
    {
      "id": "350000199512068537",
      "wechat_id": "eiqgj",
      "phone": "14166884429",
      "name": "段军",
      "index": 3
    },
    {
      "id": "630000199301100725",
      "wechat_id": "durpk",
      "phone": "14585958661",
      "name": "郑芳",
      "index": 4
    },
    {
      "id": "370000197102053353",
      "wechat_id": "uywns",
      "phone": "15628123122",
      "name": "任芳",
      "index": 5
    },
    {
      "id": "420000198104243678",
      "wechat_id": "keegs",
      "phone": "14733828249",
      "name": "段芳",
      "index": 6
    },
    {
      "id": "990000199208102867",
      "wechat_id": "bfjii",
      "phone": "14310033074",
      "name": "马勇",
      "index": 7
    },
    {
      "id": "630000201009097354",
      "wechat_id": "qkner",
      "phone": "14313920949",
      "name": "王霞",
      "index": 8
    },
    {
      "id": "610000201108312343",
      "wechat_id": "ldglx",
      "phone": "18382132410",
      "name": "彭明",
      "index": 9
    },
    {
      "id": "310000197505018332",
      "wechat_id": "tncgo",
      "phone": "15787376932",
      "name": "金伟",
      "index": 10
    },
    {
      "id": "610000201108270470",
      "wechat_id": "nsjuw",
      "phone": "15512171367",
      "name": "彭静",
      "index": 11
    },
    {
      "id": "82000019920825184X",
      "wechat_id": "ivuhc",
      "phone": "17286394622",
      "name": "董磊",
      "index": 12
    },
    {
      "id": "990000200711062781",
      "wechat_id": "wswxm",
      "phone": "14202849159",
      "name": "汤涛",
      "index": 13
    },
    {
      "id": "150000200610210611",
      "wechat_id": "vwnvl",
      "phone": "14824152287",
      "name": "汤刚",
      "index": 14
    },
    {
      "id": "340000198310233972",
      "wechat_id": "gzysq",
      "phone": "15934454235",
      "name": "林丽",
      "index": 15
    },
    {
      "id": "42000020020501126X",
      "wechat_id": "hxncn",
      "phone": "15433795836",
      "name": "姚明",
      "index": 16
    },
    {
      "id": "410000200607202219",
      "wechat_id": "tkmnm",
      "phone": "14975419206",
      "name": "朱洋",
      "index": 17
    },
    {
      "id": "230000197305281259",
      "wechat_id": "bkvlf",
      "phone": "15869843614",
      "name": "杨艳",
      "index": 18
    },
    {
      "id": "640000201702016673",
      "wechat_id": "vifef",
      "phone": "13829263258",
      "name": "雷娟",
      "index": 19
    }
  ]
}
```



### 常见使用方式-响应式数据

可以模拟真实的接口一样，根据传入的参数不同获取对应的数据

编辑页面：

```
{
  "code": function({
    _req
  }) {
    return _req.query.my_parameters ? 0 : -1;
  },
  "msg": function({
    _req
  }) {
    return _req.query.my_parameters ? "" : "缺少my_parameters参数"
  },

  "data": function({
    _req,
    Mock
  }) {
    if (_req.query.my_parameters) {
      return Mock.mock("@cname");
    } else {
      return "";
    }
  }

}
```

结果：

```
{
  "code": -1,
  "msg": "缺少my_parameters参数",
  "data": ""
}
```

