# 派趣游戏api解析

## user
- ###游客登录
      http://login.029hch.com/user/create?deviceId=56d297539312408ab57dd583f56d3d47&mobile=&source=AppStore&appId=com.handui1026.LobbyGame&t=1509691580854
      
##### 返回结果
```
{
	"status": 0,
	"msg": "\u64cd\u4f5c\u6210\u529f!",
	"data": {
		"token": "a43cfd416c59631348ac5f798bcbf251",
		"uuid": "243bb43e860f006407c77f27ead284c5",
		"deviceId": "56d297539312408ab57dd583f56d3d47",
		"nickName": "247652",
		"defaultIcon": null,
		"customIcon": null,
		"alipayBind": "0",
		"sex": "0",
		"source": "AppStore",
		"paytype": ["tfpay", "aypay", "weixin"]
	}
}
```
> 游客登录以后是生成一个token 和uuid 以及用户的相关基本信息， 也包含了 支付方式

- ###验证token
      http://login.029hch.com/frontend/user/delayedToken?token=a43cfd416c59631348ac5f798bcbf251&t=1509691643124

##### 返回结果
```
{
	"status": 0,
	"msg": "\u64cd\u4f5c\u6210\u529f!",
	"data": null
}

```

>  使用轮询对用户的token进行一个验证


## financial

- ###deposit

      http://login.029hch.com/financial/deposit/getList?token=a43cfd416c59631348ac5f798bcbf251&t=1509691581885

##### 返回结果

```
{
	"status": 0,
	"msg": "\u64cd\u4f5c\u6210\u529f!",
	"data": {
		"securityDeposit": [],
		"balance": 38033
	}
}
```
> 该api获取用户的余额信息，在大厅内会每隔一段事件请求一次， 大概是一分钟，应该是为了确保用户的余额显示正确

- ###payment

      http://login.029hch.com/financial/payment/getpayamountlist?token=a43cfd416c59631348ac5f798bcbf251&t=1509692165385

##### 返回结果

```
{
	"status": 0,
	"msg": "success",
	"data": [37, 115, 206, 611, 1012, 4555]
}
```

> 点击充值之后请求的api， 其中的data数据是推荐给用户可以充值的金额， 每次返回的结果都不同， 但差距不大

## frontend

- ###activity
          http://login.029hch.com/frontend/activity/bindMobile?token=a43cfd416c59631348ac5f798bcbf251&t=1509691582079

##### 返回结果
 ```
{
	"status": 0,
	"msg": "success",
	"data": {
		"activity_id": "14",
		"reward": "60000",
		"title": "\u7ed1\u5b9a\u624b\u673a\u6210\u529f\u5956\u52b1",
		"rewards_id": 0,
		"status": 0
	}
}
```

> 该api是在进入主页时会请求一次， 应该是用户的绑定奖励

- ###api
   1. getRedPoint (我理解为获取新消息， RedPoint小红点表示有新消息)
            http://login.029hch.com/frontend/api/getRedPoint/a43cfd416c59631348ac5f798bcbf251?uuid=undefined&t=1509691582080
##### 返回结果
```
{
	"status": 0,
	"msg": "success",
	"data": {
		"chatMessage": 0
	}
}
```

> 该api属于一个轮询， 不断的从服务器拉取新消息

2. getChatMessage
        http://login.029hch.com/frontend/api/getChatMessage/a43cfd416c59631348ac5f798bcbf251?token=a43cfd416c59631348ac5f798bcbf251&t=1509692145177

##### 返回结果
```
    {
	"status": 0,
	"msg": "success",
	"data": {
		"nick_name": "247652",
		"message": [{
			"id": "6934",
			"server_id": "44",
			"server_name": null,
			"uuid": "243bb43e860f006407c77f27ead284c5",
			"send_from": "0",
			"message": "\u6211\u613f\u7528\u6211\u4e00\u751f\u6240\u6709\uff0c\u6362\u53d6\u4f60\u7684\u4e00\u4e1d\u6e29\u67d4\u3002\u6211\u662f\u60a8\u7684\u4e13\u5c5e\u5ba2\u670d\uff0c\u8bf7\u95ee\u6709\u4ec0\u4e48\u53ef\u4ee5\u5e2e\u5230\u60a8\u7684\u5462\uff1f",
			"is_read": "1",
			"created_at": "2017-11-03 15:16:57",
			"message_type": "0"
		}, {
			"id": "6932",
			"server_id": "0",
			"server_name": null,
			"uuid": "243bb43e860f006407c77f27ead284c5",
			"send_from": "1",
			"message": "\uff1f",
			"is_read": "1",
			"created_at": "2017-11-03 15:16:52",
			"message_type": "0"
		}]
	}
}
```

> 这是我先发送了一条消息之后，对方回复了一条之后收到的返回结果。  感觉派趣没有用websocket啊， 连这个聊天还是轮询

3. sendChatMessage 
  http://login.029hch.com/frontend/api/sendChatMessage/a43cfd416c59631348ac5f798bcbf251http://login.029hch.com/frontend/api/sendChatMessage/a43cfd416c59631348ac5f798bcbf251

##### 表单数据格式

```
   {
	"status": 0,
	"msg": "\u7559\u8a00\u6210\u529f\uff0c\u611f\u8c22\u60a8\u7684\u652f\u6301\uff01",
	"data": null
    }
```

> 这是向客服发送消息的api

4. 公告的api

    http://login.029hch.com/frontend/api/getNotice?t=1509694182339

##### 返回结果

```
{
	"status": 0,
	"msg": "success",
	"data": [{
		"id": "2",
		"topic": "\u5173\u4e8e\u4e25\u5389\u6253\u51fb\u4eba\u4eba\u7c7b\u6e38\u620f\u4f5c\u5f0a\u516c\u544a",
		"notice": "\u5404\u4f4d\u4eb2\uff1a       \u4e3a\u4e86\u7ef4\u62a4\u516c\u5e73\u516c\u6b63\u7684\u6e38\u620f\u73af\u5883\uff0c\u6211\u4eec\u5c06\u4e25\u5389\u6253\u51fb\u4eba\u4eba\u7c7b\uff08\u6597\u5730\u4e3b\u3001\u9ebb\u5c06\u3001\u70b8\u91d1\u82b1\uff09\u4e00\u7c7b\u6e38\u620f\u4e2d\u8054\u5408\u4f5c\u5f0a\u5957\u5229\u884c\u4e3a\uff0c\u4e00\u7ecf\u53d1\u73b0\uff0c\u4e00\u5f8b\u5c01\u53f7\u5904\u7406\uff0c\u5e0c\u671b\u5927\u5bb6\u73cd\u60dc\u516c\u5e73\u516c\u6b63\u6e38\u620f\u73af\u5883\uff0c\u540c\u65f6\u795d\u5927\u5bb6\u6e38\u620f\u6109\u5feb\u3002",
		"type": "0",
		"time": "2017-11-03 09:56:40"
	}]
}
```

>           

## 热更新
     http://package.029hch.com/release?lang=zh-cn&appID=com.handui1026.LobbyGame&appVersion=1.1&hash=25519b8d8dfef71f4fef14e7187fdb9b&t=20171103144613713

##### 返回结果
```
{
	"code_url": "http:\/\/lobby.029hch.com\/game\/game_code_1.1.64_474.zip",
	"update_url": "http:\/\/lobby.029hch.com\/game\/"
}
```
> 派趣游戏是有热更新的， 热更新api


  
