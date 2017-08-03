# BoltHook

## Boltpard output 触发器推送

### 特点
* 以WebHook方式向外推送；
* 支持Pardpush 的 PushCard模式；
* 支持设置多个触发器；

### 设置
```javascript

  triggerName:   // 名称
  topic:         // #Slack like topic
  HookUrl:       // 目标地址，curl post 。支持设置“以短信发送=>手机号码”，但是msgType 限为 SMS
  msgType:       // 消息体类型（common、richText、link、pushCard、SMS），空为Common
  
  nocstr:        // 干扰串，非必须，按目标hook要求。Pardpush是必须要填写的
  
  eventKeys:[]    // trigger 事件点，多选多

```

### 消息体
参考Pardpush

### eventKeys
* 标准trigger事件，如：新工单；
* 流程点结束事件；
* 流程点结束事件中的自定义trigger事件，如：发现不良评价；

### 自定义trigger

一个独立的功能，在流程的流程点上设置
```javascript

  while
    Header.YPKey["key"] 满足 条件1
    or
    Header.3rdKey["key"] 满足 条件2
  then
    order.setTag("tag1")
    order.setTag("tag2")
    
    Header.YPKey["key2"] = Header.YPKey["key2"]*0.5
  end  

  // "满足": 大于 小于 between 等于 不等于 contain
  
```

## Pardpush Incoming 

### 消息体结构

* 可交互的消息推送；
* PushCard模式；
* 生成多个带token的Url；

```javascript
{ 
  /*向外兼容*/ 
  "authorName": "Stack",                          // 消息发送者的姓名，如果留空将显示为配置中的聚合标题 
  "title": "Winter is coming",                    // 聚合消息标题 
  "text": "",                                     // 聚合消息正文 
  "redirectUrl": "https://your.site",             // 跳转链接，仅在type = link 时起效
  "imageUrl": "http://your.image.url",            // 消息中可添加一张预览图片，仅在type = link 时起效 
  
  /*二级向外兼容，可能一些第三方应用不支持*/
  "authorAvatar": "http://your.image.url",        // 消息发送者的头像图片，支持图片和svg格式
  "msgType": "pushCard",                          // 支持：文本text、富文本richText、图文链接link、消息卡pushCard、voice、video
  "mediaResourceUrl",                             // 媒体型消息资源
  
  "topic":"接单"                                   // 话题归类
  
  /*Pardpush特有，type类型必须是pushCard才有效*/ 
  "callback":{                                    // 回调设置 
      "access_token" :"",                         // 回调时鉴权用Token
      "method" :"POST",                           // 回调提交模式
      "context":{                                 // 回调返回的上下文，结构自定义，非必须 
          "orderID":"",
          "contactID":""
          
          /*Pardpush need*/           
          "YPKey":""            
          "eventKeys":[]           
      }
  }, 

  "pushCard":{                                     // 消息卡设置
      "submitButtonText" :"保存",                   // 提交按钮文字
      "submittingText" :"提交中，请稍后",            // 提交中提示
      "submitCallbackUrl" :"http://your.site",     // 回调url
      "buttons":[                                  // 其他按钮组
           {
               "text":"申请移交",
               "callbackUrl" :"http://your.site/handover"
           },
           {
               "text":"取消",
               "callbackUrl" :"http://your.site/cancel"
           }
       ]
  },
  
  "actionSheet":[                                 // 交互操作界面 
     { 
          "widgetName" :"select",                 // 控件类型:text、textarea、select、datepicker、timepicker、citypicker、phone...
          "title" :"性别",                         // 显示的标题
          "subTitle" :"请选择",                    // 显示的副标题，非必须
          "valueKey" : "gender",                  // 字段名称 
          "YPKey" : "YPContactGender",            // 优豹字段名称，非必须  
          "selectOptions" : ["male","female"],    // 选择器的选择项，仅选择器可用 
          "default" : "male"                      // 默认值，非必须 
          "validator":[
              {
                  "rule":"range"
                  "value":"6,30",
                  "error":"密码长度必须大于等于6，小于30"
              }
          ]
     }
  ] 

} 
```

### 校验规则
```javascript
required:        // 必填,值为true or false，默认false
email:           // 邮箱地址
url:             // url地址
date:            // 日期
dateISO:         // ISO格式的日期(2014/08/27 或 2014-08-27)
number:          // 数字(负数，正数，小数，整数)
digits:          // 正整数

phoneCN:         // 手机号码，中国
zipCN:           // 邮政编码，中国

minlength:       // 输入字符最小长度(中文算一个字符)
maxlength:       // 输入字符最大长度(中文算一个字符)
rangelength:     // 输入字符最小，最大长度(中文算一个字符)
min:             // 数值最小值
max:             // 数值最大值
range:           // 数值最小，最大值
equalTo:         // 再次输入相同的值
```
