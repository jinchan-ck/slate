---
title: Taoooa.com接口文档

language_tabs:
  - shell
  - nodejs

<!-- toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>

includes:
  - errors -->

search: true
---

# Taoooa.com接口文档

阅读对象：广告商（需要投放效果营销广告的商户）以及媒体(拥有用户资源)需要在淘A平台投放广告或选择广告涉及的技术架构师，研发工程师，测试工程师，系统运维工程师。

# 名词解释(Dictionary)

- actionId(广告完成步骤标识)  
  广告商投放广告时，需要选择该广告所需完成步骤和完成说明，用于判定用户是否完成广告以及渠道获得的奖励金额。
- participant(广告参与者)  
  参与广告的用户信息:
  ```
  {
    participantId: String,
    mobile: String,
    email: String,
    deviceId: String
  }
  ```
  除participantId外，其余属性为可选属性。
- event(用户参与广告事件)  
  淘A平台会记录没有用户参与每个广告的事件，事件属性包括:
  ```
  {
    eventId: String,
    participantId: String,
    advertiseId: String,
    ...
  }
  ```
  广告商通过event和participant与淘A平台建立对应关系。

# 业务流程(Flow)
  ![flow](business-flow.jpg)

# 授权(Authorize)

> To authorize, use this code:

<aside class="notice">
广告商回调淘A平台API需要使用注册时分配的appkey和对应的签名算法进行授权。
</aside>

# 广告商对接(API)

广告商使用淘A平台需要对接的地方有两处，
- 广告商需要提供一个API接收广告参与者与参与事件的对应关系；
- 用户完成广告后，广告商需要通过上一步记录的对应关系，回调淘A平台更新广告参与事件状态。

**备选**
如果广告商无法回调广告完成状态，可提供查询状态接口，接口参数列表与返回值需遵循文档标准。

## 广告商记录广告参与事件(Event)API

```nodejs

```

```shell
curl -H "Content-Type: application/json"
     -X POST
     -d '{ eventId: String, participantId: String, mobile: String, email: String, deviceId: String }'
     http://host.of.advertiser/api/events
```

> 访问该接口需要返回信息如下：
```
"ok"
```

### API

`POST http://host.of.advertiser/api/events`  
`Content-Type: application/json`
### 请求参数(Body Parameters)

参数 | 一定存在 | 描述
--------- | ------- | -----------
participantId | true | 广告参与者ID
eventId | true | 广告参与事件ID
mobile | String | 广告参与者手机号
email | String | 广告参与者邮箱
deviceId | String| 广告参与者设备号

<aside class="success">
广告商需返回http状态码为200，body为"ok"；否则淘A平台会在2小时内多次调用该API。
</aside>

## 广告商回调淘A广告参与结果API

```nodejs

```

```shell
curl -H "Content-Type: application/json"
     -X POST
     -d '{ eventId: String, participantId: String, mobile: String, email: String, deviceId: String }'
     http://host.of.advertiser/api/events
```

> 淘A验证无误后将返回如下信息：
```
"ok"
```

### API

`POST http://api.taoooa.com/events`
`Content-Type: application/json`

### 请求参数(Body Parameters)

参数 | 一定存在 | 描述
--------- | ------- | -----------
participantId | true | 广告参与者ID
eventId | true | 广告参与事件ID
actionId | false | 完成步骤Id
action | true | 完成步骤说明
mobile | false | 广告参与者手机号
email | false | 广告参与者邮箱
deviceId | false| 广告参与者设备号

<aside class="success">
淘A平台会返回http状态码为200，body为"ok"。
</aside>

## 广告商提供查询广告完成状态API

```nodejs

```

```shell
curl -H "Content-Type: application/json"
     -X POST
     -d '{ eventId: String, participantId: String, mobile: String, email: String, deviceId: String }'
     http://host.of.advertiser/api/events
```

> 广告商需返回如下信息：
```json
{
  "participantId": "String",
  "eventId": "String",
  "actionId": "String",
  "action": "String",
  "mobile": "String",
  "email": "String",
  "deviceId": "String"
}
```

### API

`GET http://host.of.advertiser/api/events`

### 请求参数(Query Parameters)

参数 | 一定存在 | 描述
--------- | ------- | -----------
participantId | true | 广告参与者ID
eventId | true | 广告参与事件ID

<aside class="success">
淘A平台会返回http状态码为200，body为"event"对应json对象。
</aside>
