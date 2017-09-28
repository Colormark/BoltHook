## 说明 
* 闭包运行，只允许对当前工单的数据进行操作 
* 只支持Order的自有方法，不支持给Order扩展方法 

### Order Methods
```javascript
Order:
{
    "data":{
        "get": function (keyName, keyType){ return value; } //keyType:YoopardKey(优豹KEY),CustomKey(企业自定义KEY)，默认YoopardKey
        "set": function (keyName, value, keyType){ return bool & errorMsg; } // try catch 模式下返回是否设置成功和错误信息
        "Yoopard":{
            "orderID":"A333",
            .....
        },
        "Custom":{
            "Commission":"A333",
            .....
        },
        "awardedTeam":[
            {
              "teamID":"1234",
              "teamName":"内部客服",
              "teamType":"SelfTeam", //teamType: SelfTeam(自有团队)，Vender(服务供应商)
              "members":[
                  {
                    "roleID":"r44",
                    "roleName":"客服",
                    "companyID":"TC007", //员工号
                    "memberID":"m77",
                    "memberName":"Tony"
                  }
              ],
              childs:[]
            }
        ]
    },
    
    "tag":{
        "setted":["有问题","疑难"],
        "setTag":function(tagName){},
        "hasTag":function(tagName){ return bool; }
    },
    
    "notifaction":{
        "toMember": function(memberID,msgContent,msgChannel,msgType){},//msgChannel[Array]:sms,wechat,boltpard...
        "toConsumer": function(msgContent,msgChannel,msgType){},//msgType:text,pushcard,voice...
    },
    
    "registerTodo":function(targetTime,memo,fireEvent){}, //targetTime:目标时间Unix时间戳，memo备注，fireEvent:到期执行的事件函数
    
    "roadMap":[], //计划
  
    "currentState":{
       "stateCode":"302",
       "stateLabel":"已派单"
    },
    
    "doneSteps":[],//已完成步骤
    
    "delegate":{
        "registerEvent": function(methodEnum,EventID,EventFunction){},//EventID 自定义，需唯一；methodEnum详见下文
        "fireEvent": function(EventID){},
        "hasEvent": function(EventID){}
    },
    
    "baseInfo"：{
        orderID:"",
        orderOwner:{},
        ...
    }

}

```
#### methodEnum 
* ConsumerOnProfileUpdated(profile)
* OrderRequestOnProfileUpdated(profile)
* StateBeforeChangeTo(State)
* StateOnChanged(State)
* OrderOwnerOnChanged(owner) 
* ActionsOnSubmit(action) 
...


### 修改值
```javascript
Order.data.set("keyName", newValue);

```

### 判断与修改值
```javascript
var orderMoney = Order.data.get("OrderMoney","YoopardKey");
if (orderMoney > 1000){
  Order.data.set("Commission", orderMoney*0.1, "CustomKey");
}else{
  Order.data.set("Commission", orderMoney*0.15, "CustomKey");
}

```

### 复杂
```javascript
if (Order.tag.hasTag("疑难工单")){
  Order.notifaction.toConsumer("试用期开始","wechat","text");
  Order.registerTodo("3736237273","试用期到期提醒",function(){
      Order.notifaction.toConsumer("试用期结束","wechat","text");
  });
}

```
