## 一. 广告

#### 接口

+ `list`

  ```javascript
  requeset:
  	url: 'http://localhost:8000/api/advertisement/list',
   	method: 'GET'
  response:
  	{
      code: 0,
      data: []
    }
  ```

 请求路径：
	http://localhost:8080/green_travel/api/getAllAdvertisement.action
	

	返回数据
	{
	"code": 0,
	"data": [
	    {
	        "ad_id": 1,
	        "ad_path": "/upload/advertisement/2020-02-06/7488563750074331.jpeg",
	        "ad_date": "2020-02-01",
	        "ad_link": "https://pages.tmall.com/wow/a/act/tmall/dailygroup/147/wupr",
	        "ad_status": 0,
	        "ad_exist": 0
	    },
		.
		.
		.
		]
	}

## 二. 商品

#### 接口

+ `list`

  ```javascript
  requeset:
  	url: 'http://localhost:8000/api/merchandise/list',
    method: 'GET',
    params: {
      pageSize: 5
  		currentPage: 1
  		name: 'xxx',
    },
  response:
    {
    	code: 0,
      data: []
    }
  ```

  get方法
  请求路径
  http://localhost:8080/green_travel/api/likeCommodity.action?currentPage=1&pageSize=2&name=a
  请求参数
  currentPage=1&pageSize=2&name=a

返回数据
{
    "code": 0,
    "data": [
        {
            "com_id": 235,
            "com_name": "acsd21",
            "com_intro": "qwe12",
            "com_value": 423,
            "com_image": "wcdw",
            "create_time": "2020-02-06",
            "exist": 0,
            "com_stock": 0,
            "status": 1
        }
		.
		.
		.
		]
}

+ `total`

  ```javascript
  request:
  	url: 'http://localhost:8000/api/merchandise/total',
    method: 'GET',
    params: {
  		name: 'xxx',
    }
  response:
    {
      code: 0,
      data: 14,
    }
  ```


  get方法
  请求路径
  http://localhost:8080/green_travel/api/totalCommodity.action?name=a
  请求参数
  name=a


返回数据
{
	“code": 0,
  	"data": 15
}

+ `find`

  ```javascript
  request:
  	url: 'http://localhost:8000/api/merchandise/find',
    method: 'GET',
    params: {
      id: 1,
    },
  response:
    {
      code: 0,
      data: {
        id: 51
        name: "娃娃"
        integral: 100
        path: "/upload/merchandise/2020-02-08/7452182345126068.png"
        description: "好看的娃娃。"
        create_time: "2020-02-05T16:00:00.000Z"
        status: 1
        stock: 1
        exist: 0
      }
    }
  ```

  get方法
  请求路径
  http://localhost:8080/green_travel/api/getCommodityByid.action?id=1
  请求参数
  id=1

  返回数据
  {
    "code": 0,
    "data": {
        "com_id": 1,
        "com_name": "小吴",
        "com_intro": "1asd11切克闹",
        "com_value": 111,
        "com_image": "/upload/merchandise/2020-02-11/6662390724429095.jpeg",
        "create_time": "2020-02-13",
        "exist": 1,
        "com_stock": 111,
        "status": 1
    }
  }


+ `conversion`

  ```javascript
  request:
  	url: 'http://localhost:8000/api/merchandise/conversion',
    method: 'POST',
    Content-Type: 'application/json',
    body: {
      id: 1,
    },
  response:但是放送地方
    {
      code: 0，
     	integral: 100, // 用户当前积分
    }
  ```

    积分兑换商品接口
  进行兑换时，传用户id和商品id，然后会判断积分是否足够，在放进订单表，同时操作用户积分增减，商品表存储量减一
  传进参数  
  {
  "user_id":456654,			//用户id
  "com_id":12					//商品id
  }
  方法：post
  路径
  http://localhost:8080/green_travel/api/conversion_order.action

  response:
    {
      code: 0					//或者1
    }

## 三. 签到

#### 表结构

```javascript
CREATE TABLE `signIn` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `year` int(11) unsigned NOT NULL,
  `month` int(11) unsigned NOT NULL,
  `days` varchar(70) NOT NULL DEFAULT '',
  `user_id` int(11) unsigned NOT NULL,
  PRIMARY KEY (`id`),
  KEY `user_id` (`user_id`),
  CONSTRAINT `signin_ibfk_1` FOREIGN KEY (`user_id`) REFERENCES `user` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

#### 接口

+ `data`

  ```javascript
  request:
  	url: 'http://localhost:8000/api/signIn/data'
  	method: 'GET'
  	params: {
      user_id: 1,
    }
  response:
  	{
      code: 0,
      data: {
        id: 1,
        days: [1],
      }
    }
  ```

    方法：get

    路径:
  http://localhost:8080/green_travel/api/defaultSignin.action?year=2020&month=12&user_id=48142178
  参数
  year=2020&month=12&user_id=48142178

  返回数据
  {
    "code": 0,
    "data": {
        "days": [
            12,
            13,
            14
        ],
        "id": 8
    }
  }

+ `update`

  ```javascript
  request:
  	url: 'http://localhost:8000/api/signIn/update'
  	method: 'POST'
  	Content-Type: 'application/json'
  	body: {
      id: 8,
      user_id: 1,
      integral: 100 // 用户当前积分
    }
  response:
  	{
      code: 0
    }
  ```

      方法：post

    路径:
  http://localhost:8080/green_travel/api/Signin.action
  参数
  {
      "id": 8,
      "user_id": 1,
    }

  返回数据
  {
      code: 0
    }

  

## 四. 等级

#### 表结构

```javascript
CREATE TABLE `level` (
  `user_id` int(11) unsigned NOT NULL,
  `lv` int(11) unsigned NOT NULL DEFAULT '1',
  `growth` int(11) unsigned NOT NULL DEFAULT '0',
  UNIQUE KEY `user_id` (`user_id`),
  CONSTRAINT `level_ibfk_1` FOREIGN KEY (`user_id`) REFERENCES `user` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

#### 接口

+ `percents`

  ```
  request:
  	url: 'http://localhost:8000/api/level/percents'
  	method: 'GET'
  response:
  	{
      code: 0,
      data: [100, 0.90909, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
    }
  ```

  	请求方式	GET

  请求路径
  http://localhost:8080/green_travel/api/getAllLevel.action
  参数
  无
  返回数据
  {
    "code": 0,
    "data": [
        0.5,
        0.5,
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
        0.0,
        0.0
    ]
  }

  > 这接口要返回每个等级的占有比例，一共有15个等级。

## 五. 公益管理

#### 表结构

```javascript
CREATE TABLE `public_welfare` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `user_id` int(11) unsigned NOT NULL,
  `title` varchar(50) NOT NULL DEFAULT '',
  `integral` double unsigned NOT NULL,
  `description` varchar(300) NOT NULL DEFAULT '',
  `reason` varchar(300) NOT NULL DEFAULT '',
  `create_time` datetime NOT NULL,
  `end_time` datetime NOT NULL,
  `path` varchar(70) NOT NULL DEFAULT '',
  `destory` tinyint(11) NOT NULL DEFAULT '0',
  `exist` tinyint(11) unsigned NOT NULL DEFAULT '0',
  `status` tinyint(11) unsigned NOT NULL DEFAULT '0',
  PRIMARY KEY (`id`),
  KEY `user_id` (`user_id`),
  CONSTRAINT `public_welfare_ibfk_1` FOREIGN KEY (`user_id`) REFERENCES `user` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=utf8;
```

> 这张表是公益表，相关字段说明可以餐口后台接口文档

```javascript
CREATE TABLE `donate` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `user_id` int(11) unsigned NOT NULL,
  `public_welfare_id` int(11) unsigned NOT NULL,
  `integral` double unsigned NOT NULL,
  PRIMARY KEY (`id`),
  KEY `user_id` (`user_id`),
  KEY `public_welfare_id` (`public_welfare_id`),
  CONSTRAINT `donate_ibfk_1` FOREIGN KEY (`user_id`) REFERENCES `user` (`id`),
  CONSTRAINT `donate_ibfk_2` FOREIGN KEY (`public_welfare_id`) REFERENCES `public_welfare` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=22 DEFAULT CHARSET=utf8;
```

> 这张是捐赠表, 这张表的作用记录某某用户捐赠某某公益的碳积分，也就是关联用户表和公益表的中间表。

#### 接口

+ `save`

  ```javascript
  request:
  	url: '/api/publicWelfare/save'
  	method: 'POST'
  	body: {
      user_id: 1,
      title: "1"
      integral: "1"
      description: "123123"
      end_time: "2020-02-24 11:31"
      path: "/upload/task/2020-02-24/8373225585390902.jpeg"
    }
  response:
    {
      code: 0
    }
  ```

   请求方法	POST
  请求方式
  http://localhost:8080/green_travel/api/insertPublicWelfare.action
  传入参数
  {
      "user_id": 48142178,
      "title": "捐赠衣物活动",
      "integral": 22.1,
      "description": "人间有真情，人间有真爱",
      "end_time": "2020-02-28 11:31",
      "path": "/upload/task/2020-02-24/8373225585390902.jpeg"
    }
  返回参数
  {
    "code": 0
  }


  > 有些字段是有默认值的，这些就不需要传了。

+ `list`

  ```javascript
  request:
  	url: '/api/publicWelfare/list'
  	method: 'GET'
  response:
  	{
      code: 0,
      data: [
        id: 4
        user_id: 47880265
        title: "绿色出行"
        description: "绿色出行，你我一起行动，为社会献出一份力量，加油，噢里给。此处省略。毛绒沙发垫北欧现代简约沙发套罩欧式防滑沙发垫子坐垫沙发巾定做。毛绒沙发垫北欧现代简约沙发套罩欧式防滑沙发垫子坐垫沙发巾定做。毛绒沙发垫北欧现代简约沙发套罩欧式防滑沙发垫子坐垫沙发巾定做"
        reason: ""
        integral: 100
        create_time: "2020-02-21T14:58:23.000Z"
        end_time: "2020-02-24T04:10:00.000Z"
        path: "/upload/advertisement/2020-02-10/3497854051975744.jpeg"
        destory: 0
        exist: 0
        status: 1
        name: "wujunjia"
        donate: 137
      ]
    }
  ```

  > 数据包含三张表的内容，public_welfare, user, donate, 也就是公益的详情，公益的发布者的名字name，和公益当前的捐赠碳积分总额donate.
  >
  > 注意status的值为1，exist为0,  destory为0 也就是审核通过的且管理员没有删除的这些公益要查出来, 即使用户删除了也要查出来。
  >
  > 提示：使用派生表 


  请求方法	GET
  请求方式
  http://localhost:8080/green_travel/api/getPublicWelfare.action
  传入参数

  返回参数
  {
    "code": 0,
    "data": [
        {
            "reason": "不过关",
            "create_time": "2020-02-05 00:00:00",
            "destory": 0,
            "end_time": "2020-02-27 00:00:00",
            "description": "献出你的爱心啦",
            "title": "人间有真情，人间有真爱",
            "exist": 0,
            "path": "14/53cf3988194d422c869d1fb5f26d5aba.jpg",
            "pw_id": 2342,
            "user_id": 456654,
            "integral": 333.0,
            "name": "sky",
            "donate": 55.400000000000006,
            "status": 1
        },
  	.
  	.
  	.
  	]
  }

  

+ `find`

  ```javascript
  request:
  	url: '/api/publicWelfare/find'
  	method: 'GET'
  	params: {
      id: 1,
    }
  response:
    {
      code: 0,
      data: {
        id: 4
        user_id: 47880265
        title: "绿色出行"
        description: "绿色出行，你我一起行动，为社会献出一份力量，加油，噢里给。此处省略。毛绒沙发垫北欧现代简约沙发套罩欧式防滑沙发垫子坐垫沙发巾定做。毛绒沙发垫北欧现代简约沙发套罩欧式防滑沙发垫子坐垫沙发巾定做。毛绒沙发垫北欧现代简约沙发套罩欧式防滑沙发垫子坐垫沙发巾定做"
        reason: ""
        integral: 100
        create_time: "2020-02-21T14:58:23.000Z"
        end_time: "2020-02-24T04:10:00.000Z"
        path: "/upload/advertisement/2020-02-10/3497854051975744.jpeg"
        destory: 0
        exist: 0
        status: 1
        name: "wujunjia"
        donate: 138
      }
    }
  ```

  > 返回的数据结果与上面list接口是一样的，只是这里返回的数据只需要一条，根据id去匹配。


  请求方法	GET
  请求方式
  http://localhost:8080/green_travel/api/getPublicWelfareByid.action?id=2342
  传入参数
  id=2342
  返回参数
  {
    "code": 0,
    "data": [
        {
            "reason": "不过关",
            "create_time": "2020-02-05 00:00:00",
            "destory": 0,
            "end_time": "2020-02-27 00:00:00",
            "description": "献出你的爱心啦",
            "title": "人间有真情，人间有真爱",
            "exist": 0,
            "path": "14/53cf3988194d422c869d1fb5f26d5aba.jpg",
            "pw_id": 2342,
            "user_id": 456654,
            "integral": 333.0,
            "name": "sky",
            "donate": 55.400000000000006,
            "status": 1
        },
  	.
  	.
  	.
  	]
  }

  

+ `donate`

  ```javascript
  request:
  	url: '/api/publicWelfare/donate'
  	method: 'POST'
  	body: {
      user_id: 1
      integral: 1 // 用户捐赠的碳积分
      id: 4	// 公益表的id
    }
  response:
  	{
      code: 0
    }
  ```

  > 执行的sql有：减少用户的碳积分，查看捐赠表是否已有此捐赠记录，如果有那就跟新integral值即可，如果没有就要往捐赠表中插入此记录。
  >
  > 注意需要开启事务，出现异常需要事务回滚。

  请求方法	POST
  请求方式
  http://localhost:8080/green_travel/api/userDonate.action
  传入参数
  {
  "user_id": 123456,
    "integral":18.6,
    "id": 2342	
  }
  返回参数
  {
    "code": 0
  }


+ `rank`

  ```javascript
  request:
  	url: '/api/publicWelfare/rank'
  	method: 'GET'
  	params: {
      id: 4	// 公益表的id
      pageSize: 10
      currentPage: 1
    }
  response:
  	{
   		code: 0,
      data: [
        id: 9	// 捐赠表的id
        integral: 60
        name: "xiaoming"
      ]
    }
  ```

  > 根据公益表id去捐赠表关联查询出当前公益的捐赠者及其捐赠的碳积分。
  >
  > 注意需要按高到低排序，可以使用order by xxx desc的语法, 或者查出来之后再排序也行。

  请求方法	GET
  请求方式
  http://localhost:8080/green_travel/api/getDonate.action?id=2342&pageSize=4&currentPage=1
  传入参数
  id=2342&pageSize=4&currentPage=1
  返回参数

  {
    "code": 0,
    "data": [
        {
            "pw_id": 2342,
            "integral": 54.7,
            "name": "sky"
        },
        {
            "pw_id": 2342,
            "integral": 37.900000000000006,
            "name": "zhengzekai"
        },
        {
            "pw_id": 2342,
            "integral": 37.2,
            "name": "kkk"
        }
    ]
  }