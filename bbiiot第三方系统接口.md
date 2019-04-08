> 本文档默认采用HTTP/HTTPS传输方式，JSON数据格式，UTF-8编码，不注明默认为同步接口。

#### 历史版本

日期 | 版本号 | 负责人 | 说明
-|-|-|-
2019-04-09 | v0.3 | 布隆 | 重新整理文档
2019-04-01 | v0.2.1 | 布隆 | 接口需求初版增2.1.1
2019-03-19 | v0.2 | 郑炜 | 接口需求初版改
2019-03-18 | v0.1 | 布隆 | 起草接口需求初版

---

# 一、采集器有关接口

#### 1.1 建档请求
> 描述：采集器与电表建档关系设置

> 接口：（待补充）

> 方式：POST

> 公共请求参数：

参数 | 类型 | 是否必填 | 描述 | 示例值
-|-|-|-|-
access_token | String | 是 | 访问令牌
sign | String | 否 |签名

> 请求参数：

参数 | 类型 | 是否必填 | 描述 | 示例值
-|-|-|-|-
mode|String|是|接口方式 0-电表增加,1-电表移除
concentrator | String | 是 | 采集器编号
meters | String | 是| 电表编号 （数量：1-8）,逗号分隔|322234445555,211123335444|

> 公共响应参数：

参数 | 类型 | 是否必填 | 描述 | 示例值
-|-|-|-|-
code | String | 是 | 返回码
msg | String | 是 | 返回码描述
sign | String | 否 |签名

> 响应参数：

参数 | 类型 | 是否必填 | 描述 | 示例值
-|-|-|-|-
message | String | 是 | 返回报文
concentrator | String | 是 |采集器编号

> 响应示例：

    {
	"code": "10000",
	"msg": "Success",
	"concentrator": "06112163",
	"message": "6891170611216368"
    }
> 异常示例：

    {
	"code": "20000",
	"msg": "Without the concentrator",
    }


#### 1.2 建档结果解析接口

> 描述：采集器内部关联电表后，建档结果的报文解析

> 接口：（待补充）

> 方式：POST

> 请求参数：

参数 | 类型 | 是否必填 | 描述 | 示例值
-|-|-|-|-
access_token|String|是| 
message|String|是| 报文

> 响应参数：

参数 | 类型 | 是否必填 | 描述 | 示例值
-|-|-|-|-
code | String | 是 | 返回码
msg | String | 是 | 返回码描述

#### 1.3 离线抄表请求接口
> 描述：根据采集器、电表编号来获取离线抄表的指令？

> 待补充...

#### 1.4 离线抄表数据解析接口

> 描述：解析离线抄表数据

> 待补充...

#### 1.5 抄表定时任务设置接口

> 描述：抄表定时任务设置，1小时、2小时等

> 待补充...

# 二、任务相关接口

#### 2.1 任务下发接口

> 描述：召测、拉闸、合闸的任务下发

> 接口：（待补充）

> 公共请求参数：

参数 | 类型 | 是否必填 | 描述 | 示例值
-|-|-|-|-
access_token | String | 是 | 访问令牌
sign | String | 否 |签名

> 请求参数：

参数 | 类型 | 是否必填 | 描述 | 示例值
-|-|-|-|-
mode|String|是|接口方式 0-数据召测,1-合闸,2-拉闸
meter | String | 是| 电表编号 
task_id | String | 是 | 任务id|

> 响应参数：

参数 | 类型 | 是否必填 | 描述 | 示例值
-|-|-|-|-
code | String | 是 | 返回码|10000
msg | String | 是 | 返回码描述

> 响应示例：

    {
	"code": "10000",
	"msg": "Success"
    }
    
> 异常示例：

    {
	"code": "20000",
	"msg": "Unfinished task",
    }
    
    
#### 2.2 任务结果回调（异步）

> 描述：异步将任务结果回调

> 接口：/api/bbi/callback

> 方式：POST

> 请求参数：

参数 | 类型 | 是否必填 | 描述 | 示例值
-|-|-|-|-
sign | String | 是 |签名 | 见附4
meter | String | 是 | 电表编号 |
task_id | String | 是 | 任务编号 |
data | JSON | 是 | 电表数据 | 见附1

> 请求示例：

    {
        "sign" : "xxx",
        "meter" : "111800000490",
        "task_id" : "aaa-bbb-ccc-ddd",
        "data" : {
            "yxzt" : "xx",
            "sjzt" : "xxxx",
            "jdqzt" : "1",
            ...
            // 其他附1所定义的...
        }
    }

> 返回：（该回调接口不返回任何数据，可根据HTTP的状态码来判断是否回调成功，200为成功）

#### 2.3 电表数据主动推送

> 描述：主动推送采集器上报的电表数据

> 接口：/api/bbi/push

> 请求参数：

参数 | 类型 | 是否必填 | 描述 | 示例值
-|-|-|-|-
sign | String | 是 |签名 | 见附4
meter | String | 是 | 电表编号 |
data_id | String | 是 | 数据标识 | UUID，保证唯一即可
data | JSON | 是 | 电表数据 | 见附1。多条数据时则是JSON数组结构

> 请求示例：
    
    {
        "sign" : "xxx",
        "meter" : "111800000490",
        "data_id" : "aaa-bbb-ccc-ddd",
        "data" : [
            {
                "yxzt" : "xx",
                "sjzt" : "xxxx",
                "jdqzt" : "1",
                 ...
                 // 其他附1所定义的...
            },
            {
                "yxzt" : "yyy",
                "sjzt" : "yyyy",
                "jdqzt" : "0",
                ...
            },
            ...
        ]
    }

> 返回：（同样地，该接口也不返回任何数据，可通过HTTP状态码来判断是否调用成功，200表示成功）

# 三、其他

#### 3.1 获取token接口

> 描述：获取token

> 接口：（待补充）

> 方式：POST

> 请求头：Authorization:Basic dGVzdDp0ZXN0

> 请求参数：

参数 | 类型 | 是否必填 | 描述 | 示例值
-|-|-|-|-
username | String | 是 | 访问账号
password | String | 是 |访问密码
scope    |String|是|鉴权作用域 |server
grant_type | String | 是| 验证模式|password

> 响应参数：

参数 | 类型 | 是否必填 | 描述 | 示例值
-|-|-|-|-
access_token | String | 是 | token|
token_type | String | 是 | 返回码描述
refresh_token | String | 是 | 用于刷新token
expires_in | String | 是 | token剩余时效（秒）
scope | String | 是 | 鉴权作用域
> 响应示例：

    {
    "access_token": "eded1764-27f4-4e27-84a6-454b8e437299",
    "token_type": "bearer",
    "refresh_token": "5f82f582-a19c-43df-8aea-6a7fed1eb9b1",
    "expires_in": 42177,
    "scope": "server"
}


#### 3.2 刷新token接口

> 描述：刷新token

> 接口：（待补充）

> 方式：POST

> 请求头：Authorization:Basic dGVzdDp0ZXN0

> 请求参数：

参数 | 类型 | 是否必填 | 描述 | 示例值
-|-|-|-|-
scope    |String|是|鉴权作用域 |server
grant_type | String | 是| 验证模式| refresh_token
refresh_token | String | 是 |刷新token

> 响应参数：

参数 | 类型 | 是否必填 | 描述 | 示例值
-|-|-|-|-
access_token | String | 是 | token|
token_type | String | 是 | 返回码描述
refresh_token | String | 是 | 用于刷新token
expires_in | String | 是 | token剩余时效（秒）
scope | String | 是 | 鉴权作用域
> 响应示例：

    {
    "access_token": "eded1764-27f4-4e27-84a6-454b8e437299",
    "token_type": "bearer",
    "refresh_token": "5f82f582-a19c-43df-8aea-6a7fed1eb9b1",
    "expires_in": 42177,
    "scope": "server"
    }

---

# 附1：电表数据结构定义

参数 | 类型 | 是否必填 | 描述 | 示例值
-|-|-|-|-
sjsj | String | 是 | 数据时间
yxzt | String | 是 |运行状态(未启用)
sjzt | String | 是 |事件告警
jdqzt | String | 是 |继电器状态|0-合闸、1-拉闸
axdy | number | 是 |A相电压(V)
bxdy | number | 是 |B相电压(V)
cxdy | number | 是 |C相电压(V)
axdl | number | 是 |A相电流(A)
bxdl | number | 是 |B相电流(A)
cxdl | number | 是 |C相电流(A)
sydl | number | 是 |剩余电流(A)
zyggl | number | 是 |总有功功率(kW)
axyggl | number | 是 |A相有功功率(kW)
bxyggl | number | 是 |B相有功功率(kW)
cxyggl | number | 是 |C相有功功率(kW)
zwggl | number | 是 |总无功功率(kvar)
axwggl | number | 是 |A相无功功率(kvar)
bxwggl | number | 是 |B相无功功率(kvar)
cxwggl | number | 是 |C相无功功率(kvar)
zglys | number | 是 |总功率因数
axglys | number | 是 |A相功率因数
bxglys | number | 是 |B相功率因数
cxglys | number | 是 |C相功率因数
zxygzdl | number | 是 |正向有功总电量(kWh)
zxygzdl1 | number | 是 |正向有功费率1电量(kWh)
zxygzdl2 | number | 是 |正向有功费率2电量(kWh)
zxygzdl3 | number | 是 |正向有功费率3电量(kWh)
zxygzdl4 | number | 是 |正向有功费率3电量(kWh)
axwd | number | 是 |A相温度(℃)
bxwd | number | 是 |B相温度(℃)
cxwd | number | 是 |C相温度(℃)
lxwd | number | 是 |零线温度(℃)
hjwd | number | 是 |环境温度(℃)

> 示例：

    {
    	"zwggl": 0.0803,
    	"zyggl": 1.6414,
    	"bxdy": 0,  
    	"bxwggl": 0,
    	"bxyggl": 0,
    	"cxglys": 0,
    	"axdl": 6.452,
    	"cxdl": 0,
    	"bxglys": 0,
    	"meterNo": "621712300001",  
    	"sydl": 6.488,
    	"sjsj": "2021-09-28 09:04:00",
    	"zglys": 1,
	    "yxzt": "00000000",
    	"axdy": 254.7,
    	"zxygzdl": 8.96,
    	"zxygzdl1": 8.96,
    	"sjzt": "0",
    	"cxdy": 0,
    	"zxygzdl3": 0,
    	"zxygzdl2": 0,
    	"axglys": 1,
    	"zxygzdl4": 0,
    	"cxwggl": 0,
    	"axwd": 24.9,
	    "cxyggl": 0,
    	"bxdl": 0,
    	"axyggl": 1.6414,
    	"axwggl": 0.0803,
    	"hjwd": 20.2
    }

# 附2：事件警告定义

|code|事件|
|-|-|
|1001|A相过压|
|1002|B相过压
|1003|C相过压
|1004|A相过流
|1005|B相过流
|1006|C相过流
|1007|A相起弧
|1008|B相起弧
|1009|C相起弧
|1010|A相过温度
|1011|B相过温度
|1012|C相过温度
|1013|零线过温度
|1014|环境温度超
|1015|恶性负载
|1016|剩余电流
|1017|三相电流不平衡
|1018|电压不平衡
|1022|温度故障
|1023|剩余电流故障


# 附3：业务码定义

code（返回码）| msg（返回码描述）|解决方案
-|-|-
10000|接口调用成功，调用结果请参考具体的API文档所对应的业务返回参数
20001|表计已存在召测任务

# 附4：sign签名规则

sign签名规则过程：将task_id或data_id、meter(电表编号)、username(用户名)、password(密码)、secretKey(密钥)，这五个数据按顺序组成一个字符串，然后用sha1加密。

> 伪代码：

String taskId = "aaaa";

String meter = "11180000490";

String username = "xxxx";

String password = "yyyy";

String secretKey = "zzzzz";

String code = taskId + meter + username + password + secretKey;

String sign = sha1(code);