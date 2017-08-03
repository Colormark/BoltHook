# BoltHook

# Boltpard output

# Pardpush Incoming
```javascript
{ 
/*向外兼容*/ 
  "authorName": "Stack",                          // 消息发送者的姓名，如果留空将显示为配置中的聚合标题 
  "title": "Winter is coming",                    // 聚合消息标题 
  "text": "",                                     // 聚合消息正文 
  "redirectUrl": "https://talk.ai/site",          // 跳转链接 
  "imageUrl": "http://your.image.url",            // 消息中可添加一张预览图片 
  
/*Pardpush 特有*/ 

  "callback":{                                    // 消息中可添加一张预览图片 
    "access_token" :"",                           // 消息中可添加一张预览图片 
    "method" :"POST"                              // 消息中可添加一张预览图片 
  }, 
  
  "actionSheet":[                                 // 消息中可添加一张预览图片 
     { 
          "widgetName" :"selector",               // 控件类型 
          "valueKey" : "gender"                   // 字段名称 
          "YPKey" : "YPContactGender"             // 优豹字段名称，非必须  
          "selectOptions" : ["male","female"]     // 选择器的选择项，仅选择器可用 
          "default" : "male"                      // 默认值，非必须 
  ], 



} 
```
