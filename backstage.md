## 一. 商品管理

#### 表结构

```sql
CREATE TABLE `merchandise` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(50) NOT NULL DEFAULT '',
  `integral` double unsigned NOT NULL,
  `description` varchar(300) NOT NULL DEFAULT '',
  `path` varchar(70) NOT NULL DEFAULT '',
  `create_time` date NOT NULL,
  `exist` int(11) unsigned NOT NULL DEFAULT '0',
  `stock` int(11) unsigned NOT NULL DEFAULT '0',
  `status` int(11) unsigned NOT NULL DEFAULT '1',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

> exist字段是判断此数据是否删除，0是未删除，1是已删除。
>
> stock字段是库存。
>
> status字段是判断该商品是否上架， 0表示已下架，1表示已上架。

#### 接口

> 所有接口返回数据中需要有一个code字段，0表示成功，1表示失败

+ `upload`:

  ```javascript
  requeset:
  	url: 'http://localhost:3001/api/upload/merchandise'
    method: 'POST'
    Content-Type: 'multipart/form-data'
    body: {
      name: 'merchandise',
      merchandise: (binary)
    }
  response:
  	{
      code: 0,
  		path: "/upload/merchandise/2020-02-11/6531908940093578.jpeg"
    }
  ```

  post方法
  请求路径
  http://localhost:8080/green_travel/api/pictureUtil.action
  请求参数
  {
     “name”: 'merchandise',
     “merchandise”: (binary)
  }

返回数据
{
    "code": 0,
  	"path": "/upload/advertisement/2020-02-11/6531908940093578.jpeg"
}

+ `add`

  ```javascript
  request:
  	url: 'http://localhost:3000/api/merchandise/add'
    method: 'POST'
    Content-Type: 'application/json'
    body: {
      name: "沙发"
      description: "舒适的沙发。"
      integral: 100
      status: 1
      stock: 10
      path: "/upload/merchandise/2020-02-11/6531908940093578.jpeg"
      create_time: "2020-02-11"
    }
  response:
  	{
      code: 0
    }
  ```


  + `add` 商品添加
    post方法
    请求路径
    http://localhost:8080/green_travel/api/insertCommodity.action
    请求参数
    {
    "name": "小郑",
    "description": "1asd11切克闹6666",
    "status": 1,
    "stock": 111,
    "integral": 111,
    "path": "/upload/merchandise/2020-02-11/6662390724429095.jpeg"
    }
    返回数据
    {
    "code": 0
    }


+ `delete`

  ```javascript
  request:  
  	url: '/api/merchandise/delete'
    method: 'POST'
    Content-Type: 'application/json'
    body: {
      ids: [50]
    }
  response:
   	{
      code: 0
    }
  ```

  > 删除可以是批量删除，当id只有一个时就是单个删除，单个或多个删除是通用的。

  + `delete` 商品删除
    post方法
    请求路径
    http://localhost:8080/green_travel/api/deleteAllCommodity.action
    请求参数
    {
    "ids":[44,212]
    }
    返回数据
    {
    "code": 0
    }

+ update`

  ```javascript
  request
  	url: '/api/merchandise/edit'
    method: 'POST'
    Content-Type: 'application/json'
    body: {
  		id: 58
  		name: "111"
  		description: "111"
      status: 1
      stock: 111
      integral: 111
      path: "/upload/merchandise/2020-02-11/6662390724429095.jpeg"
    }
  response:
    {
      code: 0
    }
  ```

+ `update` 商品更新/修改
  post方法
  请求路径
  http://localhost:8080/green_travel/api/updateCommodity.action
  请求参数
  {
  "id": 1,
  	"name": "小吴",
  	"description": "1asd11切克闹",
    "status": 1,
    "stock": 111,
    "integral": 111,
    "path": "/upload/merchandise/2020-02-11/6662390724429095.jpeg"
  }

返回数据
{
	"code": 0
}

+ `list`

  ```javascript
  request:
  	url: '/api/merchandise/list'
    method: 'GET'
    params: {
      pageSize: 6,
      currentPage: 1,
      name: ''
    }
  response:
  	{
      code: 0,
      data: [],
    }
  ```

  > name是模糊查询

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
  	url: '/api/merchandise/total'
    method: 'GET',
  params: {
      name: '',
    },
  response:
   	{
      code: 0,
  		data: 15
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

  > 返回根据模糊查询后的数据总条数

## 二. 广告管理

#### 表结构

```javascript
CREATE TABLE `advertisement` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `path` varchar(70) NOT NULL DEFAULT '',
  `create_time` date NOT NULL,
  `outside_link` text,
  `status` int(11) unsigned DEFAULT '0',
  `exist` int(11) unsigned DEFAULT '0',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

> outside_link：外链，也就是当用户点击这张图片是可以跳转到该广告页面
>
> status: 是否启用，0停用，1启用
>
> exist：是否删除, 0未删除，1已删除

#### 接口

+ `upload`

  ```javascript
  requeset:
  	url: 'http://localhost:3001/api/upload/advertisement'
    method: 'POST'
    Content-Type: 'multipart/form-data'
    body: {
      name: 'advertisement',
      advertisement: (binary)
    }
  response:
  	{
      code: 0,
  		path: "/upload/advertisement/2020-02-11/6531908940093578.jpeg"
    }
  
  ```

  post方法
  请求路径
  返回数据
  同商品图片一样

  > 上传图片的时候需要新增一条广告数据，数据的除了时间，图片路径外都是默认的。

+ `update`

  ```javascript
  request:  
  	url: '/api/advertisement/edit'
    method: 'POST'
    Content-Type: 'application/json'
    body: {
      id: 100,
      status: 0,
      outside_link: "https://pages.tmall.com/wow/a/act/tmall/dailygroup/147/wupr",
      path: "/upload/advertisement/2020-02-06/7488563750074331.jpeg"
    }
  response:
  	{
      code: 0
    }
  
  ```

  post方法
  请求路径
  http://localhost:8080/green_travel/api/updateAdvertisement.action
  请求参数
  {
      "id": 1,
      "status": 0,
      "outside_link": "https://pages.tmall.com/wow/a/act/tmall/dailygroup/147/wupr",
      "path":"/upload/advertisement/2020-02-06/7488563750074331.jpeg"
  }

返回数据
{
	"code": 0
}

+ `delete`: 

  ```javascript
  request:
  	url: '/api/advertisement/delete'
    method: 'POST'
    Content-Type: 'application/json'
    body: {
      ids: [1,2,3]
    }
  response:
  	{
      code: 0
    }
  
  ```

  post方法
  请求路径
  http://localhost:8080/green_travel/api/deleteAllAdvertisement.action
  请求参数
  {
  "ids":[44,212]
  }
  {
  "code": 0
  }

+ `list`

  ```javascript
  request:  
  	url: '/api/advertisement/list'
    method: 'GET'
    params: {
      pageSize: 10,
      currentPage: 1,
      create_time: {"max":"","min":""},
      status: -1
    }
  response:
  	{
      code: 0,
      data: []
    }
  
  ```

  > status: -1是全部，0停用，1启用
  >
  > create_time: 起始日期和结束日期

  get方法
  请求路径
  http://localhost:8080/green_travel/api/getAdvertisement.action?currentPage=1&pageSize=4&status=1&create_time={"max":"2020-02-10","min":"2020-02-03"}
  请求参数
  currentPage=1&pageSize=4&status=1&create_time={"max":"2020-02-10","min":"2020-02-03"}

返回数据
{
    "code": 0,
    "data": [
        {
            "ad_id": 5,
            "ad_path": "1",
            "ad_date": "2020-02-03",
            "ad_link": "1",
            "ad_status": 1,
            "ad_exist": 0
        },
		.
		.
		.
		]
}

+ `total`

  ```javascript
  request:
  	url: '/api/advertisement/total',
    method: 'GET',
    params: {
      create_time: {"max":"","min":""},
      status: -1
    },
  response:
   {
     	code: 0,
  		data: 7
   }
  
  ```

  + `total`
    get方法
    请求路径
    http://localhost:8080/green_travel/api/totalAdvertisement.action?status=1&create_time={"max":"2020-02-10","min":"2020-02-03"}
    请求参数
    status=1&create_time={"max":"2020-02-10","min":"2020-02-03"}

返回数据

{
	“code": 0,
  	"data": 15
}


## 三. auth

#### 表结构

```javascript
CREATE TABLE `user` (
  `id` int(11) unsigned NOT NULL,
  `name` varchar(11) NOT NULL DEFAULT '',
  `integral` double unsigned DEFAULT '0',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

```

#### 接口

+ `/api/login`

  > 当点击登录页面的图标时，会请求后台接口，然后重定向到github的登录页面。
  >
  > 登录成功后重定向到http://localhost:3000页面

+ `/api/user`

  > 请求登录着的信息。有登录返回code为0，还有user信息，如果没登录返回code为1
  >
  > response: 
  >
  > {
  >
  > ​	code: 0,
  >
  > ​	user: {
  >
  > ​		id: 1,
  >
  > ​		name: 'xxx',
  >
  > ​		level: 1,
  >
  > ​		growth: 100
  >
  > ​	}
  >
  > }


  请求方式	GET
  请求路径
  http://localhost:8080/green_travel/api/LoginUser.action

  返回数据
  {
    "code": 0,
    "user": {

  >​		"id": 1,
  >
  >​		"name": 'xxx',
  >
  >​		"level": 1,
  >
  >​		"growth": 100
  >
  >​	}
  >}

## 四. 兑换

#### 表结构

```javascript
CREATE TABLE `conversion` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `merchandise_id` int(11) unsigned NOT NULL,
  `serial_number` char(40) NOT NULL DEFAULT '',
  `user_id` int(11) unsigned NOT NULL,
  `quantity` int(11) unsigned NOT NULL,
  `create_time` datetime NOT NULL,
  `exist` tinyint(11) unsigned NOT NULL DEFAULT '0',
  PRIMARY KEY (`id`),
  KEY `merchandise_id` (`merchandise_id`),
  KEY `user_id` (`user_id`),
  CONSTRAINT `conversion_ibfk_1` FOREIGN KEY (`merchandise_id`) REFERENCES `merchandise` (`id`),
  CONSTRAINT `conversion_ibfk_2` FOREIGN KEY (`user_id`) REFERENCES `user` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;

```

> serial_number: 编号，可以使用mysql的uuid函数生成，或者自己创建一串随机字符。

#### 接口

+ `list`

  ```javascript
  request:
  	url: '/api/conversion/list'
  	method: 'GET'
  	params: {
      pageSize: 6
  		currentPage: 1
  		name: ''
    }
  response:
  	{
      code: 0,
      data: [
        {
          id: 1
          merchandise_id: 44
          serial_number: "GREEN-TRAVEL-2557179658511197"
          user_id: 47880265
          quantity: 1
          create_time: "2020-02-17T16:00:00.000Z"
          exist: 0
          m_path: "/upload/merchandise/2020-02-08/5124512917972819.jpeg"
          m_name: "小电煮锅"
          m_status: 1
          m_integral: 20.05
          m_stock: 1
          m_description: "奥林格(AOLINGE) ZG-304 学生锅宿舍小电煮锅寝室家用煮面煲汤迷你小功率电热火锅多功能用锅煎炒煮一体1.2L"
          u_name: "wujunjia"
        }
      ]
    }
  
  ```

  请求方式	GET
  请求路径
  http://localhost:8080/green_travel/api/getAllorder.action?name=ze&pageSize=3&currentPage=2
  参数
  name=ze&pageSize=3&currentPage=2
  返回数据
  {
    "code": 0,
    "data": [
        {
            "u_name": "zhengzekai",
            "quantity": 1,
            "create_time": "2020-02-22 00:00:00",
            "com_id": 44,
            "serial_number": "green_travel-C6X2KFzoBN",
            "com_image": "/upload/merchandise/2020-02-14/53cf3988194d422c869d1fb5f26d5aba.jpg",
            "exist": 0,
            "user_id": 48142178,
            "com_intro": "小可爱",
            "com_value": 32.0,
            "order_id": 304348827,
            "com_name": "蓝猫",
            "status": 1,
            "com_stock": 221
        },
  	]
  }

+ `total`

  ```javascript
  request:
  	url: '/api/conversion/total'
  	method: 'GET'
  	params: {
      name: ''
    }
  response:
  	{
      code: 0,
      data: 10,
    }
  
  ```

  	请求方式	GET
  	

  请求路径
  http://localhost:8080/green_travel/api/getTotalorder.action?name=zzz
  参数
  name=zzz
  返回数据
  {
    "code": 0,
    "data": 0
  }

+ `delete`

  ```javascript
  request:
  	url: '/api/conversion/delete'
  	method: 'POST'
  	body: {
      ids: [1]
    }
  
  ```

  	请求方式	POST
  	

  请求路径
  http://localhost:8080/green_travel/api/deleteOrders.action	
  参数
  body: {
      ids: [1]
    }
  返回数据

  {
    "code": 0
  }

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

> Reason: 审核不通过时备注的原因
>
> Create_time: 用户发布公益的时间
>
> End_time: 公益的结束时间
>
> Destroy: 给用户删除用的
>
> Exist：给后台管理员删除用的
>
> Status: 审核的状态： 0是待审核，1是通过审核，2是未通过审核

### 接口

+ `list`

  ```javascript
  request:
     url: '/api/publicWelfare/list',
     methods: 'GET'
     params: {
       	pageSize: 6,
      	currentPage: 1,
        name: ''
     }
  response:
  	{
      code: 0,
      data: [{
        id: 3
        user_id: 47880265
        title: "绿色出行"
        description: "绿色出行，你我一起行动，为社会献出一份力量，加油，噢里给。此处省略。毛绒沙发垫北欧现代简约沙发套罩欧式防滑沙发垫子坐垫沙发巾定做。毛绒沙发垫北欧现代简约沙发套罩欧式防滑沙发垫子坐垫沙发巾定做。毛绒沙发垫北欧现代简约沙发套罩欧式防滑沙发垫子坐垫沙发巾定做"
        reason: ""
        integral: 100
        create_time: "2020-02-21T14:58:23.000Z"
        end_time: "2020-02-21T15:00:00.000Z"
        path: "/upload/advertisement/2020-02-08/6601061291089769.jpeg"
        destory: 0
        exist: 0
        status: 0
        name: "wujunjia"
      }]
    }
  
  ```

  > 返回的数据包含两种表的内容，public_welfare和user

   请求方法	GET
  请求方式
  http://localhost:8080/green_travel/api/getAllPublicWelfare.action?currentPage=1&pageSize=2&name=

  请求参数
  currentPage=1&pageSize=2&name=

  返回数据
  {
    "code": 0,
    "data": [
        {
            "reason": "",
            "create_time": "2020-02-05 00:00:00",
            "destory": 0,
            "end_time": "2020-02-29 00:00:00",
            "description": "捐出你的爱",
            "title": "献出你的爱心吧",
            "exist": 0,
            "path": "14/53cf3988194d422c869d1fb5f26d5aba.jpg",
            "pw_id": 32312,
            "user_id": 123456,
            "integral": 22.0,
            "name": "kkk",
            "status": 0
        }
    ]
  }

+ `total`

  ```javascript
  request:
  	url: '/api/publicWelfare/total'
  	methods: 'GET'
  	params: {
      name: ''
    }
  response:
  	{
      code: 0,
      data: 10
    }
  
  ```

  请求方法	GET
  请求方式
  http://localhost:8080/green_travel/api/totalPublicWelfare.action?name=zz

  请求参数
  name=zz

  返回数据
  {
    "code": 0,
    "data": 1
  }

+ `delete`

  ```javascript
  request:
  	url: '/api/publicWelfare/delete'
  	method: 'POST'
  	body: {
      ids: [1]
    }
  
  ```

  请求方法	POST
  请求方式
  http://localhost:8080/green_travel/api/deletePublicWelfare.action

  请求参数
  {
  "ids":[2342,32312]
    }

  返回数据
  {
    "code": 0
  }

+ `audit`

  ```javascript
  request:
  	url: '/api/publicWelfare/audit'
  	method: 'POST'
  	body: {
      status: 1
      id: 3
      reason: ""
    }
  
  ```

   请求方法	POST
  请求方式
  http://localhost:8080/green_travel/api/updateStatus.action

  {
  "id": 2342,
  "status":1,
  "reason":"不过关"

  }

  返回数据
  {
    "code": 0
  }

  > 页面有审核的按钮有三个，一个是通过，一个是不通过，一个是重审，无论点击那个按钮都是发这个请求，status，reason的值会做相应的处理。