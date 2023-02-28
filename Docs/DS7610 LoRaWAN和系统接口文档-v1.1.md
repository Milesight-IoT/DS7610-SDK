

[English](https://github.com/Milesight-IoT/DS7610-SDK/blob/main/Docs/DS7610%20LoRaWAN%26System%20API%20Document-V1.1.md) | 中文

# DS7610 LoRaWAN和系统接口文档

* [DS7610 LoRaWAN和系统接口文档](#ds7610-lorawan和系统接口文档)
   * [1.前提条件](#1前提条件)
      * [1.1 部署Android开发环境](#11-部署android开发环境)
      * [1.2 导入项目依赖](#12-导入项目依赖)
   * [2. 实现流程](#2-实现流程)
      * [2.1 主动调用](#21-主动调用)
         * [2.1.1 基本的调用方式](#211-基本的调用方式)
         * [2.1.2 主动调用的常量](#212-主动调用的常量)
      * [2.2 订阅](#22-订阅)
   * [3. 接口实现](#3-接口实现)
      * [3.1 LoRa系统设置](#31-lora系统设置)
         * [3.1.1 启用LoRa网关](#311-启用lora网关)
         * [3.1.2 禁用LoRa网关](#312-禁用lora网关)
         * [3.1.3 获取LoRa网关的可用状态](#313-获取lora网关的可用状态)
         * [3.1.4 获取LoRa网关的运行状态](#314-获取lora网关的运行状态)
         * [3.1.5 LoRa频段参数](#315-lora频段参数)
         * [3.1.6 LoRa频率列表](#316-lora频率列表)
         * [3.1.7 LoRa速率列表](#317-lora速率列表)
         * [3.1.8 LoRa设置更新](#318-lora设置更新)
      * [3.2 profile设置](#32-profile设置)
         * [3.2.1 profile查询](#321-profile查询)
         * [3.2.2 增加profile](#322-增加profile)
         * [3.2.3 修改profile](#323-修改profile)
         * [3.2.4 删除profile](#324-删除profile)
      * [3.3 组播设置](#33-组播设置)
         * [3.3.1 组播概览](#331-组播概览)
         * [3.3.2 新增组播](#332-新增组播)
         * [3.3.3 查询组播](#333-查询组播)
         * [3.3.4 修改组播](#334-修改组播)
         * [3.3.5 删除组播](#335-删除组播)
      * [3.4 节点设置](#34-节点设置)
         * [3.4.1 节点概览](#341-节点概览)
         * [3.4.2 查询节点](#342-查询节点)
         * [3.4.3 新增节点](#343-新增节点)
            * [3.4.3.1 通过EUI添加节点设备](#3431-通过eui添加节点设备)
            * [3.4.3.2 多参数添加节点设备](#3432-多参数添加节点设备)
            * [3.4.3.3 NFC添加节点设备](#3433-nfc添加节点设备)
         * [3.4.4 修改节点](#344-修改节点)
         * [3.4.5 删除节点](#345-删除节点)
      * [3.5 数据查看](#35-数据查看)
      * [3.6 下行指令发送](#36-下行指令发送)
      * [3.7 组播指令发送](#37-组播指令发送)
      * [3.8 系统设置](#38-系统设置)
         * [3.8.1 硬件版本](#381-硬件版本)
         * [3.8.2 SN](#382-sn)
         * [3.8.3 软件版本](#383-软件版本)
         * [3.8.4 获取USB类型](#384-获取usb类型)
         * [3.8.5 设置USB类型](#385-设置usb类型)
      * [3.9 灯带设置](#39-灯带设置)
         * [3.9.1 灯带设置红色](#391-灯带设置红色)
         * [3.9.2 灯带设置蓝色](#392-灯带设置蓝色)
         * [3.9.3 灯带设置绿色](#393-灯带设置绿色)
         * [3.9.4 灯带设置自定义色值](#394-灯带设置自定义色值)
         * [3.9.5 关闭灯带](#395-关闭灯带)
         * [3.9.6 设置灯带亮度1](#396-设置灯带亮度1)
         * [3.9.7 设置灯带亮度2](#397-设置灯带亮度2)
         * [3.9.8 设置灯带亮度3](#398-设置灯带亮度3)
      * [3.10 屏幕亮度设置](#310-屏幕亮度设置)
         * [3.10.1 获取屏幕亮度](#3101-获取屏幕亮度)
         * [3.10.2 熄屏](#3102-熄屏)
         * [3.10.3 亮屏](#3103-亮屏)
      * [3.11 获取产品型号名称](#311-获取产品型号名称)



<center><big>文档修订记录</big></center>

| 版本 | 变更时间   | 备注                                         |
| ---- | ---------- | -------------------------------------------- |
| V1.0 | 2022.12.19 | 初稿                                         |
| V1.1 | 2023.1.31  | 新增灯带和屏幕亮度设置，支持获取产品型号名称 |



## 1.前提条件

### 1.1 部署Android开发环境

下载安装Android studio，配置好开发环境。

**注意：本接口文档是配合DS7610使用的，除文档内接口外，其它部分请直接参考Android标准接口：https://developer.android.com/guide**

### 1.2 导入项目依赖

只要确认DS7610的固件版本是72.0.0.5-r2以上即可，DS系统配置功能使用Android的组件Provider和Receiver来实现，不需要导入外部包，LoRa功能可参考LoRaWanGateWayDemo源码。



## 2. 实现流程

### 2.1 主动调用

#### 2.1.1 基本的调用方式

```java
resolver = getActivity().getContentResolver();
uri = Uri.parse(Constant.DS_SYSTEM_URI);
Bundle bundle = resolver.call(uri, Constant.LORA_CONFIG_QUERY_SERVER_NAME, null, null);
int code = bundle.getInt(Constant.BUNDLE_CODE, -1);
if (code == 0) {
    String jsonStr = bundle.getString(Constant.BUNDLE_CONTENT);
    ...
}
```

#### 2.1.2 主动调用的常量

```java
public class Constant {
    public static final String BUNDLE_CODE = "code";//返回code
    public static final String BUNDLE_CONTENT = "content";//返回内容
    public static final String DS_SYSTEM_URI = "content://com.ds.system.provider";//provider的URI
    public static final String SUBSCRIBE_ACTION = "com.ds.system.mqtt.broadcast";//订阅广播的Action
    
    public static final String ENABLE_LORA_NS = "enable_lora_ns";//启用LoRa网关
    public static final String DISABLE_LORA_NS = "disable_lora_ns";//禁用LoRa网关
    public static final String GET_LORA_NS_STATE = "get_lora_ns_state";//获取LoRa网关可用状态
    public static final String GET_LORA_NS_RUN_STATE = "get_lora_ns_run_state";//获取LoRa网关运行状态
    public static final String LORA_CONFIG_QUERY_SERVER_NAME = "lora_config_query_server_name";//获取LoRa网关频段参数
    public static final String LORA_CONFIG_QUERY_FREQUENCY = "lora_config_query_frequency";//获取LoRa网关频率列表
    public static final String LORA_CONFIG_QUERY_DATA_RATES = "lora_config_query_data_rates";//获取LoRa网关速率列表
    public static final String LORA_CONFIG_UPDATE = "lora_config_update";//LoRa网关设置更新
    public static final String GET_LORA_DATA_WITH_NFC = "get_lora_data_with_nfc";//NFC一键添加LoRa节点时需要获取的网关信息
    
    public static final String DS_PROFILES_QUERY = "ds_profiles_query";//查询profile
    public static final String DS_PROFILES_ADD = "ds_profiles_add";//增加profile
    public static final String DS_PROFILES_UPDATE = "ds_profiles_update";//修改profile
    public static final String DS_PROFILES_DELETE = "ds_profiles_delete";//删除profile
    
    public static final String DS_MULTICAST_GROUPS_ADD = "ds_multicast_groups_add";//新增组播
    public static final String DS_MULTICAST_GROUPS_QUERY = "ds_multicast_groups_query";//查询组播
    public static final String DS_MULTICAST_GROUPS_UPDATE = "ds_multicast_groups_update";//修改组播
    public static final String DS_MULTICAST_GROUPS_DELETE = "ds_multicast_groups_delete";//删除组播
    
    public static final String DS_DEVICES_QUERY = "ds_devices_query";//节点设备查询
    public static final String DS_DEVICES_ADD = "ds_devices_add";//新增节点设备
    public static final String DS_DEVICES_UPDATE = "ds_devices_update";//修改节点设备
    public static final String DS_DEVICES_DELETE = "ds_devices_delete";//删除节点设备
    
    public static final String DS_PACKETS_QUERY = "ds_packets_query";//查询节点设备上下行数据流
 
    public static final String MQTT_SEND_TX_DATA = "send_tx_data";//Mqtt发送tx信息
    
    public static final String MQTT_SEND_MULTICAST_DATA = "send_multicast_data";//组播数据发送
    
    public static final String GET_HARDWARE = "get_hardware";//硬件版本
    public static final String GET_SN = "get_sn";//sn
    public static final String GET_SYSTEM_VERSION = "get_system_version";//软件版本
    public static final String GET_USB_TYPE = "get_usb_type";//获取USB类型
    public static final String SET_USB_TYPE = "set_usb_type";//设置USB类型
}
```

### 2.2 订阅

> 除了主动调用外还有设备信息的订阅，可以在主界面或者是Application注册广播接收器：如下

```java
myReceiver = new MyReceiver();
IntentFilter intentFilter = new IntentFilter();
intentFilter.addAction(Constant.SUBSCRIBE_ACTION);
registerReceiver(myReceiver, intentFilter);
```

> 对应的广播接收器如下：

```java
public class MyReceiver extends BroadcastReceiver {

    @Override
    public void onReceive(Context context, Intent intent) {
        // TODO: This method is called when the BroadcastReceiver is receiving
        Bundle bundle = intent.getExtras();
        if (bundle != null) {
            String topic = bundle.getString("topic");
            String message = bundle.getString("message");
            EventBus.getDefault().post(new MqttEvent(topic,message));
        }
    }
}
```

> 订阅主题代码：

```java
contentResolver.call(uri, Constant.MQTT_SUBSCRIBE_RX, urDeviceVo.getDevEUI(), null);
```

> 可用订阅的主题如下：

```java
public static final String MQTT_SUBSCRIBE_ALL = "mqtt_subscribe_all";//Mqtt监听所有订阅（组播除外）
public static final String MQTT_UNSUBSCRIBE_ALL = "mqtt_unsubscribe_all";//Mqtt取消所有订阅（组播除外）
public static final String MQTT_SUBSCRIBE_JOIN = "mqtt_subscribe_join";//Mqtt监听Join订阅
public static final String MQTT_UNSUBSCRIBE_JOIN = "mqtt_unsubscribe_join";//Mqtt取消Join订阅
public static final String MQTT_SUBSCRIBE_RX = "mqtt_subscribe_rx";//Mqtt监听RX
public static final String MQTT_UNSUBSCRIBE_RX = "mqtt_unsubscribe_rx";//Mqtt取消监听Rx
public static final String MQTT_SUBSCRIBE_ACK = "mqtt_subscribe_ack";//Mqtt监听Ack
public static final String MQTT_UNSUBSCRIBE_ACK = "mqtt_unsubscribe_ack";//Mqtt取消监听Ack
public static final String MQTT_SUBSCRIBE_ERROR = "mqtt_subscribe_error";//Mqtt监听Error
public static final String MQTT_UNSUBSCRIBE_ERROR = "mqtt_unsubscribe_error";//Mqtt取消监听Error
public static final String MQTT_SUBSCRIBE_TX = "mqtt_subscribe_tx";//Mqtt监听Tx
public static final String MQTT_UNSUBSCRIBE_TX = "mqtt_unsubscribe_tx";//Mqtt取消监听Tx
```

> 订阅后通过广播接收器收到的返回值如下：

```
topic=application/1/device/24e124136c379261/rx, message={"applicationID":"1","applicationName":"app","deviceName":"24e124136c379261","devEUI":"24e124136c379261","rxInfo":[{"mac":"24e124fffef59d60","time":"2022-12-08T15:02:18Z","rssi":-39,"loRaSNR":8,"name":"Local Gateway","latitude":0,"longitude":0,"altitude":0}],"txInfo":{"frequency":916800000,"dataRate":{"modulation":"LORA","bandwidth":125,"spreadFactor":10},"adr":true,"codeRate":"4/5"},"fCnt":1,"fPort":85,"data":"/wv//wEB","time":"2022-12-08T15:02:18Z"}
```

## 3. 接口实现

### 3.1 LoRa系统设置

> 这一小节主要是LoRa相关的设置接口，包括开关LoRa网关，获取LoRa网关的开关状态、运行状态，设置LoRa网关的频段、频率、速率等
>
> 注意：LoRa网关的开启有个过程，并不是一开就生效，所以开启后并不能马上知道开启成功还是失败，要通过LoRa网关运行状态才能知道

#### 3.1.1 启用LoRa网关

```java
public void setLoraNsEnable() {
    resolver.call(uri, ENABLE_LORA_NS, null, null);//不需要关注返回值
}
```

#### 3.1.2 禁用LoRa网关

```java
public void setLoraNsDisable() {
    resolver.call(uri, DISABLE_LORA_NS, null, null);//不需要关注返回值
}
```

#### 3.1.3 获取LoRa网关的可用状态

> 获取的是设置LoRa网关的状态，并不是实际LoRa网关的运行状态

```java
public boolean isLoraNsEnable() {
    Bundle bundle = resolver.call(uri, GET_LORA_NS_STATE, null, null);
    int code = bundle.getInt(BUNDLE_CODE, -1);
    boolean isEnable = LORA_STATE_ENABLE == code;
    return isEnable;
}
```

#### 3.1.4 获取LoRa网关的运行状态

```java
public int getLoraRunState() {
    Bundle bundle = resolver.call(uri, GET_LORA_NS_RUN_STATE, null, null);
    int code = bundle.getInt(BUNDLE_CODE, -1);
    return code;
}
```

> 返回值意义如下：

```java
public static final int LORA_STATE_CLOSED = -103;//LoRa网关关闭成功
public static final int LORA_STATE_FAIL = -104;//LoRa网关开启/关闭失败
public static final int LORA_STATE_OPENED = -105;//LoRa网关开启成功
```

#### 3.1.5 LoRa频段参数

> LoRa网关参数通过调用如下接口获取：

```java
Bundle input = new Bundle();
input.putString("servName", servNameVo.getServName());
Bundle bundle = resolver.call(uri, Constant.LORA_CONFIG_QUERY_SERVER_NAME, null, input);
```

> 返回值示例如下：

```json
{
    "servName":"IN865",
    "gatewayID":"24E124FFFEF59D60",
    "rxFrequence":"865.4025",
    "servNames":[
        "EU868",
        "IN865",
        "RU864"
    ],
    "serverAddress":"Embeded Network Server",
    "rxDataRate":1
}
```

| 接口参数      | SDK选项    | 备注                                       |
| ------------- | ---------- | ------------------------------------------ |
| gatewayID     | R string   | 只读信息，gatewayEUI                       |
| servName      | R/W string | 信道方案                                   |
| rxFrequence   | R/W int    | 频率                                       |
| rxDataRate    | R/W int    | 速率                                       |
| servNames     | R string   | 只读，当前可选频段信息，获取频段信息时返回 |
| serverAddress | R/W string | 当前只支持内置NS `Embeded Network Server`  |

#### 3.1.6 LoRa频率列表

> 通过servName获取频率列表

```java
Bundle input = new Bundle();
input.putString("servName", servNameVo.getServName());
Bundle bundle = resolver.call(uri, Constant.LORA_CONFIG_QUERY_FREQUENCY, null, input);
```

> 返回值示例如下：

```json
{
    "channels":[
        {
            "channel":0,
            "frequency":"923.2"
        },
        {
            "channel":1,
            "frequency":"923.4"
        },
        {
            "channel":2,
            "frequency":"0"
        }
    ],
    "minFrequency":"915",
    "maxFrequency":"928",
    "defaultFrequency":"923.2"
}
```

| 接口参数         | SDK选项 | 备注     |
| ---------------- | ------- | -------- |
| channel          | int     | 通道     |
| frequency        | string  | 频率     |
| minFrequency     | string  | 最小频率 |
| maxFrequency     | string  | 最大速率 |
| defaultFrequency | string  | 默认频率 |

#### 3.1.7 LoRa速率列表

> 通过servName和rxFrequence获取速率列表

```java
Bundle input = new Bundle();
input.putString("servName", servNameVo.getServName());
input.putString("rxFrequence", servNameVo.getRxFrequence());
Bundle bundle = resolver.call(uri, Constant.LORA_CONFIG_QUERY_DATA_RATES, null, input);
```

> 返回值示例如下：

```json
{
    "defaultDr":2,
    "datarates":[
        {
            "datarate":0,
            "spreadfactor":10
        },
        {
            "datarate":1,
            "spreadfactor":9
        },
        {
            "datarate":2,
            "spreadfactor":8
        },
        {
            "datarate":3,
            "spreadfactor":7
        }
    ]
}
```

| 接口参数     | SDK选项 | 备注     |
| ------------ | ------- | -------- |
| datarate     | int     | 速率     |
| spreadfactor | int     | 扩频因子 |
| defultDr     | int     | 默认速率 |

#### 3.1.8 LoRa设置更新

```java
Bundle updateBundle = resolver.call(uri, Constant.LORA_CONFIG_UPDATE, JSON.toJSONString(servNameVo), null);
int updateCode = updateBundle.getInt(Constant.BUNDLE_CODE, -1);
```

> 其中入参数据结构如下

```java
{
    "gatewayID": "24E124FFFEF59F32",
    "rxFrequence": 868100000,
    "rxDataRate": 4,
    "servName": "EU868"
}
```



> 其中servNameVo的参数如下：

| 接口参数    | SDK选项    | 备注                 |
| ----------- | ---------- | -------------------- |
| gatewayID   | R string   | 只读信息，gatewayEUI |
| servName    | R/W string | 频段信息             |
| rxFrequence | R/W int    | 频率                 |
| rxDataRate  | R/W int    | 速率与扩频因子       |

### 3.2 profile设置

> profile概览

| 接口参数       | SDK选项    | 备注                          |
| -------------- | ---------- | ----------------------------- |
| name           | R/W string | 设备配置文件名称              |
| rxDataRate2    | R/W int    | 窗口2接收速率                 |
| rxFreq2        | R/W int    | 窗口2接收频率                 |
| supportsJoin   | R/W bool   | 入网方式，true:OTAA false:ABP |
| supportsClassB | R/W bool   | 工作方式是否⽀持CLASS B       |
| supportsClassC | R/W bool   | 工作方式是否⽀持CLASS C       |

> rxDataRate2不同频段对应的默认值及选项

|                | **CN470**       | **EU868**       | **IN865**       | **RU864**       | **KR920**       | **US915**        | **AU915**        | **AS923-1**     | **AS923-2**     | **AS923-3**     | **AS923-4**     |
| -------------- | --------------- | --------------- | --------------- | --------------- | --------------- | ---------------- | ---------------- | --------------- | --------------- | --------------- | --------------- |
| **默认值**     | DR0 (SF12,125k) | DR0 (SF12,125k) | DR2 (SF10,125k) | DR0 (SF12,125k) | DR0 (SF12,125k) | DR8 (SF12,500k)  | DR8 (SF12,500k)  | DR2 (SF10,125k) | DR2 (SF10,125k) | DR2 (SF10,125k) | DR2 (SF10,125k) |
| **下拉框选项** | DR0 (SF12,125k) | DR0 (SF12,125k) | DR0 (SF12,125k) | DR0 (SF12,125k) | DR0 (SF12,125k) | DR8 (SF12,500k)  | DR8 (SF12,500k)  | DR0 (SF12,125k) | DR0 (SF12,125k) | DR0 (SF12,125k) | DR0 (SF12,125k) |
|                | DR1 (SF11,125k) | DR1 (SF11,125k) | DR1 (SF11,125k) | DR1 (SF11,125k) | DR1 (SF11,125k) | DR9 (SF11,500k)  | DR9 (SF11,500k)  | DR1 (SF11,125k) | DR1 (SF11,125k) | DR1 (SF11,125k) | DR1 (SF11,125k) |
|                | DR2 (SF10,125k) | DR2 (SF10,125k) | DR2 (SF10,125k) | DR2 (SF10,125k) | DR2 (SF10,125k) | DR10 (SF10,500k) | DR10 (SF10,500k) | DR2 (SF10,125k) | DR2 (SF10,125k) | DR2 (SF10,125k) | DR2 (SF10,125k) |
|                | DR3 (SF9,125k)  | DR3 (SF9,125k)  | DR3 (SF9,125k)  | DR3 (SF9,125k)  | DR3 (SF9,125k)  | DR11 (SF9,500k)  | DR11 (SF9,500k)  | DR3 (SF9,125k)  | DR3 (SF9,125k)  | DR3 (SF9,125k)  | DR3 (SF9,125k)  |
|                | DR4 (SF8,125k)  | DR4 (SF8,125k)  | DR4 (SF8,125k)  | DR4 (SF8,125k)  | DR4 (SF8,125k)  | DR12 (SF8,500k)  | DR12 (SF8,500k)  | DR4 (SF8,125k)  | DR4 (SF8,125k)  | DR4 (SF8,125k)  | DR4 (SF8,125k)  |
|                | DR5 (SF7,125k)  | DR5 (SF7,125k)  | DR5 (SF7,125k)  | DR5 (SF7,125k)  | DR5 (SF7,125k)  | DR13 (SF7,500k)  | DR13 (SF7,500k)  | DR5 (SF7,125k)  | DR5 (SF7,125k)  | DR5 (SF7,125k)  | DR5 (SF7,125k)  |

> rxFreq2不同频段对应的默认值及选项

|                    | **CN470**   | **EU868** | **IN865** | **RU864** | **KR920**   | **US915**       | **AU915**       | **AS923-1** | **AS923-2** | **AS923-3** | **AS923-4** |
| ------------------ | ----------- | --------- | --------- | --------- | ----------- | --------------- | --------------- | ----------- | ----------- | ----------- | ----------- |
| **默认值（MHz)**   | 505.3       | 869.525   | 866.55    | 869.1     | 921.9       | 923.3           | 923.3           | 923.2       | 921.4       | 916.6       | 917.3       |
| **输入范围（MHz)** | 500.3~509.7 | 863~870   | 865~867   | 864~870   | 920.9~923.3 | **923.3~927.5** | **923.3~927.5** | 915~928     | 915~928     | 915~928     | 915~928     |

#### 3.2.1 profile查询

> profile查询代码示例：

```java
Bundle bundle = contentResolver.call(uri, Constant.DS_PROFILES_QUERY, null, null);
String json = bundle.getString(Constant.BUNDLE_CONTENT);
int code = bundle.getInt(Constant.BUNDLE_CODE, -1);
```

> 注意，示例中的返回值是默认创建不可修改、删除的：

```json
{
    "totalCount":8,
    "result":[
        {
            "supportsClassB":false,
            "supportsClassC":false,
            "rxDataRate2":2,
            "rxFreq2":866550000,
            "supportsJoin":false,
            "name":"ClassA-ABP"
        },
        {
            "supportsClassB":false,
            "supportsClassC":false,
            "rxDataRate2":2,
            "rxFreq2":866550000,
            "supportsJoin":true,
            "name":"ClassA-OTAA"
        },
        {
            "supportsClassB":true,
            "supportsClassC":false,
            "rxDataRate2":2,
            "rxFreq2":866550000,
            "supportsJoin":false,
            "name":"ClassB-ABP"
        },
        {
            "supportsClassB":true,
            "supportsClassC":false,
            "rxDataRate2":2,
            "rxFreq2":866550000,
            "supportsJoin":true,
            "name":"ClassB-OTAA"
        },
        {
            "supportsClassB":false,
            "supportsClassC":true,
            "rxDataRate2":2,
            "rxFreq2":866550000,
            "supportsJoin":false,
            "name":"ClassC-ABP"
        },
        {
            "supportsClassB":false,
            "supportsClassC":true,
            "rxDataRate2":2,
            "rxFreq2":866550000,
            "supportsJoin":true,
            "name":"ClassC-OTAA"
        },
        {
            "supportsClassB":true,
            "supportsClassC":true,
            "rxDataRate2":2,
            "rxFreq2":866550000,
            "supportsJoin":false,
            "name":"ClassCB-ABP"
        },
        {
            "supportsClassB":true,
            "supportsClassC":true,
            "rxDataRate2":2,
            "rxFreq2":866550000,
            "supportsJoin":true,
            "name":"ClassCB-OTAA"
        }
    ]
}
```

#### 3.2.2 增加profile

> 增加profile代码示例：

```java
Bundle bundle = contentResolver.call(uri, Constant.DS_PROFILES_ADD, text, null);
String json = bundle.getString(Constant.BUNDLE_CONTENT);
```

> 增加profile入参示例：

```json
{
    "name":"ClassCB-ABP1",
    "profile":{
        "supportsClassB":true,
        "supportsClassC":true,
        "rxDataRate2":2,
        "rxFreq2":866550000,
        "supportsJoin":false
    }
}
```

> 返回示例：

```json
{"profileID":"2725d684-7404-4a68-9520-790d366a990b"}
```

#### 3.2.3 修改profile

> 注意：默认创建的 profile 不允许修改，即3.2.1查询返回示例中的profile是不可修改的，但是每次修改频段都会更新默认配置
>
> 修改profile代码示例如下，其中factor为profileName，如果修改的是name，则必须是未修改前的name：

```java
Bundle input = new Bundle();
input.putString("factor", name);
Bundle bundle = contentResolver.call(uri, Constant.DS_PROFILES_UPDATE, content, input);
int code = bundle.getInt(Constant.BUNDLE_CODE, -1);
String json = bundle.getString(Constant.BUNDLE_CONTENT);
```

> 修改profile示例：

```json
{
    "name":"ClassCB-ABP1",
    "profile":{
        "supportsClassB":true,
        "supportsClassC":true,
        "rxDataRate2":3,
        "rxFreq2":866550000,
        "supportsJoin":false
    }
}
```

#### 3.2.4 删除profile

> 注意：默认创建的 profile 不允许更新和删除，即3.2.1查询返回示例中的profile是不可删除的

```java
Bundle input = new Bundle();
input.putString("factor", name);
Bundle bundle = contentResolver.call(uri, Constant.DS_PROFILES_DELETE, "", input);
int code = bundle.getInt(Constant.BUNDLE_CODE, -1);
String json = bundle.getString(Constant.BUNDLE_CONTENT);
```

### 3.3 组播设置

#### 3.3.1 组播概览

组播一般用于向一组设备批量下发一条配置或控制指令，DS7610支持两种组播方式：

非D2D组播：即通过LoRaWAN发送组播指令给一组组播地址、组播网络密钥和组播应用密钥相同的设备，该功能仅适用于支持组播功能的Class B和Class C设备。

D2D组播：通过Milesight D2D协议发送组播指令给一组D2D密钥相同的设备，该功能仅适用于可作为Milesight D2D被控端的设备。DS7610默认创建了一个Milesight D2D组播，改组播组不允许更新和删除，每次修改频段都会更新默认配置。

> 当 mcAddr 设置为 "" 且 mcKey 不为 "" 时，为D2D配置，mcNwkSKey mcAppSKey 均为空
> 非D2D配置时，密钥参数为 mcNwkSKey mcAppSKey，mcKey 可为空

| 接口参数  | SDK选项           | 备注                              |
| --------- | ----------------- | --------------------------------- |
| name      | R/W string        | 组播名                            |
| id        | R string          | 组播ID                            |
| mcAddr    | R/W string(HEX8)  | 组播地址                          |
| mcKey     | R/W string(HEX32) | 组播密钥(D2D密钥)                 |
| mcNwkSKey | R/W string(HEX32) | 组播网络密钥                      |
| mcAppSKey | R/W string(HEX32) | 组播应用密钥                      |
| frequency | R/W int           | 组播频率，0时使用默认的速率和频率 |
| dr        | R/W int           | 组播速率                          |

#### 3.3.2 新增组播

> 新增非D2D组播

```java
JSONObject jsonObject = new JSONObject();
jsonObject.put("name", firstInput);
jsonObject.put("mcAddr", secondInput);
jsonObject.put("mcNwkSKey", thirdInput);
jsonObject.put("mcAppSKey", fourthInput);
jsonObject.put("frequency", Integer.valueOf(frequency));
jsonObject.put("dr", Integer.valueOf(dr));
Bundle bundle = contentResolver.call(uri, Constant.DS_MULTICAST_GROUPS_ADD, jsonObject.toJSONString(), null);
```

> 新增D2D组播

```java
JSONObject jsonObject = new JSONObject();
jsonObject.put("name", firstInput);
jsonObject.put("mcKey", secondInput);
jsonObject.put("frequency", Integer.valueOf(frequency));
jsonObject.put("dr", Integer.valueOf(dr));
Bundle bundle = contentResolver.call(uri, Constant.DS_MULTICAST_GROUPS_ADD, jsonObject.toJSONString(), null);
```

#### 3.3.3 查询组播

```java
Bundle bundle = contentResolver.call(uri, Constant.DS_MULTICAST_GROUPS_QUERY, null, null);
int code = bundle.getInt(Constant.BUNDLE_CODE, -1);
String jsonStr = bundle.getString(Constant.BUNDLE_CONTENT);
```

> 返回值

```
{
    "totalCount":2,
    "result":[
        {
            "id":"8eccb9b6-6c46-449c-881c-bc9ce0ec7ca0",
            "name":"D2D",
            "mcAddr":"",
            "mcNwkSKey":"",
            "mcAppSKey":"",
            "dr":2,
            "frequency":866550000,
            "mcKey":"5572404c696e6b4c6f52613230313823"
        },
        {
            "id":"5b04efe9-87ba-4ef7-b37f-c5a005d91053",
            "name":"yyyd",
            "mcAddr":"",
            "mcNwkSKey":"",
            "mcAppSKey":"",
            "dr":2,
            "frequency":866550000,
            "mcKey":"5572404c696e6b4c6f52613230313823"
        }
    ]
}
```

#### 3.3.4 修改组播

> 默认创建的 D2D组 不允许更新和删除，每次修改频段都会更新默认配置

```java
input.putString("factor", vo.getName());
JSONObject jsonObject = new JSONObject();
jsonObject.put("name", mEtName.getText().toString().trim());
jsonObject.put("mcAddr", mEtMcAddr.getText().toString().trim());
jsonObject.put("mcNwkSKey", mEtMcNwkSKey.getText().toString().trim());
jsonObject.put("mcAppSKey", mEtMcAppSKey.getText().toString().trim());
jsonObject.put("mcKey", mEtMcKey.getText().toString().trim());
jsonObject.put("frequency", Integer.valueOf(frequency));
jsonObject.put("dr", Integer.valueOf(dr));
Bundle bundle = contentResolver.call(uri, Constant.DS_MULTICAST_GROUPS_UPDATE, jsonObject.toJSONString(), input);
String json = bundle.getString(Constant.BUNDLE_CONTENT);
```

#### 3.3.5 删除组播

> 默认创建的 D2D组 不允许更新和删除，每次修改频段都会更新默认配置

```java
Bundle input = new Bundle();
input.putString("factor", vo.getName());
Bundle bundle = contentResolver.call(uri, Constant.DS_MULTICAST_GROUPS_DELETE, "", input);
```

### 3.4 节点设置

#### 3.4.1 节点概览

| 接口参数     | SDK选项           | 备注                                               |
| ------------ | ----------------- | -------------------------------------------------- |
| name         | R/W string        | device name(仅用于显示，以deveui为主)              |
| devEUI       | R/W string(HEX16) | 设备EUI，仅创建时可写，创建后禁⽌修改              |
| profileName  | R/W string        | 设备配置文件名称                                   |
| appKey       | R/W string(HEX32) | 应用程序秘钥，仅 OTAA 时可写                       |
| devAddr      | R/W string(HEX8)  | 设备地址，仅 ABP 时可写                            |
| appSKey      | R/W string(HEX32) | 应用程序会话秘钥，仅 ABP 时可写                    |
| nwkSKey      | R/W string(HEX32) | 网络会话秘钥，仅 ABP 时可写                        |
| fCntUp       | R/W int/string    | 上行帧计数，仅 ABP 时可写，创建为0，修改时保留原值 |
| fCntDown     | R/W int/string    | 下行帧计数，仅 ABP 时可写，创建为0，修改时保留原值 |
| active       | R bool            | 入网状态                                           |
| supportsJoin | R bool            | 入网方式，true:OTAA false:ABP，根据查询的值设置    |

#### 3.4.2 查询节点

> 列出节点设备列表

```java
private void initDeviceList() {
    Bundle bundle = contentResolver.call(uri, Constant.DS_DEVICES_QUERY, null, null);
    int code = bundle.getInt(Constant.BUNDLE_CODE, -1);
    if (code == 0) {
        String json = bundle.getString(Constant.BUNDLE_CONTENT);
        List<UrDeviceVo> list = getList(json);
        ...
    }
}
```

> 节点设备通过devEUI条件查询单个节点设备数据

```java
Bundle input = new Bundle();
input.putString("factor", urDeviceVo.getDevEUI());
bundle = contentResolver.call(uri, Constant.DS_DEVICES_SEARCH, "", input);
String json = bundle.getString(Constant.BUNDLE_CONTENT);
int code = bundle.getInt(Constant.BUNDLE_CODE, -1);
```

> 返回值json如下：

```json
{
    "devTotalCount":3,
    "deviceResult":[
        {
            "devEUI":"24E124535B312156",
            "name":"em300",
            "profileName":"ClassA-OTAA",
            "fCntUp":0,
            "fCntDown":0,
            "appKey":"5572404c696e6b4c6f52613230313823",
            "devAddr":"",
            "appSKey":"",
            "nwkSKey":"",
            "active":false
        },
        {
            "devEUI":"1123456789012345",
            "name":"dddrrr",
            "profileName":"ClassA-OTAA",
            "fCntUp":0,
            "fCntDown":0,
            "appKey":"5572404c696e6b4c6f52613230313823",
            "devAddr":"",
            "appSKey":"",
            "nwkSKey":"",
            "active":false
        },
        {
            "devEUI":"1234567890123456",
            "name":"ggguh",
            "profileName":"ClassA-OTAA",
            "fCntUp":0,
            "fCntDown":0,
            "appKey":"5572404c696e6b4c6f52613230313823",
            "devAddr":"",
            "appSKey":"",
            "nwkSKey":"",
            "active":false
        }
    ]
}
```



#### 3.4.3 新增节点

##### 3.4.3.1 通过EUI添加节点设备

> 通过EUI新增节点代码示例：

```java
Map<String, Object> map = new HashMap<>();
map.put("name", firstInput);
map.put("devEUI", secondInput);
map.put("appKey", defaultKey);
Bundle bundle = contentResolver.call(uri, Constant.DS_DEVICES_ADD, JSON.toJSONString(map), null);
```

##### 3.4.3.2 多参数添加节点设备

> 多参数添加节点设备的调用方法一样，只是增加了参数

```java
private void save() {
    Map<String, Object> map = new HashMap<>();
    map.put("name", name);
    map.put("devEUI", devEUI);
    map.put("devAddr", devAddr);
    map.put("supportsJoin", selectProfile.isSupportsJoin());
    if (selectProfile.isSupportsJoin()) {
        map.put("appKey", appKey);
    } else {
        map.put("appSKey", appSKey);
        map.put("nwkSKey", nwkSkey);
    }
    map.put("applicationID", "1");
    map.put("fCntUp", 0);
    map.put("fCntDown", 0);
    map.put("profileID", selectProfile.getProfileID());
    map.put("skipFCntCheck", false);
    Bundle bundle;
    bundle = contentResolver.call(uri, Constant.DS_DEVICES_ADD, JSON.toJSONString(map), null);
    ...
}
```

##### 3.4.3.3 NFC添加节点设备

> 一键添加设备的好处是用户使用体验较好，不需要输入设备参数即可添加成功，但是开发者要调用的接口较多
>
> 1.先调用Constant.GET_LORA_DATA_WITH_NFC和Constant.DS_PROFILES_QUERY获取数据
>
> 2.再调用NFC方法DsNfcManager.getInstance().oneKeyAddDsDevice(MifareUltralight mifareUltralight, String profileData, String loraData, Progress progress) 
>
> 3.如果节点设备和LoRa网关的信道方案不一致的情况下，需要先修改节点设备的信道方案，信道方案修改成功后，再跑一遍1~3的流程

```java
Bundle nfcBundle = getContentResolver().call(uri, Constant.GET_LORA_DATA_WITH_NFC, null, null);//返回NFC添加设备时需要的网关信息参数
int nfcCode = nfcBundle.getInt(Constant.BUNDLE_CODE, -1);
String nfcInfo = nfcBundle.getString(Constant.BUNDLE_CONTENT);
Bundle profileBundle = getContentResolver().call(uri, Constant.DS_PROFILES_QUERY, null, null);//返回profile列表
int profileCode = profileBundle.getInt(Constant.BUNDLE_CODE, -1);
String profile = profileBundle.getString(Constant.BUNDLE_CONTENT);
if (profileCode == 0 && nfcCode == 0) {
    //调用这个接口一键添加设备
    nfcResult = DsNfcManager.getInstance().oneKeyAddDsDevice(mifareUltralight, profile, nfcInfo, position -> {
        fragment.setProgress(position);
    });
    if (nfcResult.getCode() == NfcResult.SUCCESS) {
        //一键添加成功后还要再调用这个接口添加设备
        Bundle bundle = getContentResolver().call(uri, Constant.DS_DEVICES_ADD, nfcResult.getJson(), null);
        ...
    }
}
```

#### 3.4.4 修改节点

```java
private void save() {
    ...
    map.put("name", name);
    map.put("description", name);
    map.put("devEUI", devEUI);
    map.put("devAddr", devAddr);
    map.put("supportsJoin", selectProfile.isSupportsJoin());
    //根据选择的profile判断是appKey还是appSKey
    if (selectProfile.isSupportsJoin()) {
        map.put("appKey", appKey);
    } else {
        map.put("appSKey", appSKey);
        map.put("nwkSKey", nwkSkey);
    }
    map.put("applicationID", "1");
    map.put("fCntUp", 0);
    map.put("fCntDown", 0);
    map.put("profileID", selectProfile.getProfileID());
    map.put("skipFCntCheck", false);
    Bundle bundle;
    Bundle input = new Bundle();
    input.putString("factor", urDeviceVo.getDevEUI());
    bundle = contentResolver.call(uri, Constant.DS_DEVICES_SEARCH, "", input);
    String json = bundle.getString(Constant.BUNDLE_CONTENT);
    int code = bundle.getInt(Constant.BUNDLE_CODE, -1);
    if (code == 0) {
        JSONObject originObj = JSON.parseObject(json);
        int devTotalCount = originObj.getInteger("devTotalCount");
        if (devTotalCount > 0) {
            JSONArray jsonArray = originObj.getJSONArray("deviceResult");
            UrDeviceVo urDeviceVo = jsonArray.getObject(0, UrDeviceVo.class);
            if (urDeviceVo != null) {
                map.put("fCntUp", urDeviceVo.getFCntUp());
                map.put("fCntDown", urDeviceVo.getFCntDown());
            }
        }
        bundle = contentResolver.call(uri, Constant.DS_DEVICES_UPDATE, JSON.toJSONString(map), input);
        json = bundle.getString(Constant.BUNDLE_CONTENT);
        code = bundle.getInt(Constant.BUNDLE_CODE, -1);
        if (code == 0) {
            finish();
        } else {
            Toast.makeText(getApplicationContext(), json, Toast.LENGTH_LONG).show();
        }
    } else {
        Toast.makeText(getApplicationContext(), json, Toast.LENGTH_LONG).show();
    }
}
```

#### 3.4.5 删除节点

```java
Bundle input = new Bundle();
input.putString("factor", urDeviceVo.getDevEUI());
Bundle bundle = contentResolver.call(uri, Constant.DS_DEVICES_DELETE, "", input);
int code = bundle.getInt(Constant.BUNDLE_CODE, -1);
if (code == 0) {
    //成功
} else {
    //失败
}
```

### 3.5 数据查看

> 查看所有的节点上报数据，返回值字段如下

| 接口参数     | SDK选项           | 备注                                                         |
| ------------ | ----------------- | ------------------------------------------------------------ |
| type         | R string          | JnAcc-同意入网数据包<br/>JnReq-入网请求数据包<br/>UpUnc-上行未确认数据包<br/>UpCnf-上行确认数据包-来自网络请求的ACK响应<br/>DnUnc-下行未确认包<br/>DnCnf-下行确认包--来自网络请求的ACK响应 |
| payloadHex   | R string(HEX)     | 负载                                                         |
| fPort        | R int/string      | 端口                                                         |
| fCnt         | R int/string      | 帧计数                                                       |
| time         | R string          | 固定为GMT时间                                                |
| rssi/loraSNR | R string(int/int) | 信号值                                                       |

> 调用代码如下：

```
Bundle input = new Bundle();
input.putInt("offset", 0);//起始值
input.putInt("limit", 10);//一次获取数量，最多500个
Bundle bundle = contentResolver.call(uri, Constant.DS_PACKETS_QUERY, null, input);
```

```json
{"packets":[{"frequency":923300000,"power":"27","immediately":"false","dataRate":"SF10BW500","modulation":"LORA","bandwidth":500,"spreadFactor":10,"bitRate":0,"codeRate":"4/5","gatewayMac":"24E124FFFEF59D60","timeSinceGPSEpoch":"","timestamp":4141465704,"rssi":"-","loraSNR":"-","devEUI":"24E124136C379261","time":"2022-12-08T15:04:09Z","type":"DnUnc","fCnt":6,"devAddr":"060328C3","adr":"true","adrAckReq":"false","ack":"false","mic":"c84067ad","appEUI":"24E124C0002A0001","fPort":"","size":"0","payloadBase64":"","payloadHex":"","enqueue":false,"classType":"Class A"},{"frequency":923300000,"power":"27","immediately":"false","dataRate":"SF10BW500","modulation":"LORA","bandwidth":500,"spreadFactor":10,"bitRate":0,"codeRate":"4/5","gatewayMac":"24E124FFFEF59D60","timeSinceGPSEpoch":"","timestamp":4123548704,"rssi":"-","loraSNR":"-","devEUI":"24E124136C379261","time":"2022-12-08T15:03:52Z","type":"DnUnc","fCnt":5,"devAddr":"060328C3","adr":"true","adrAckReq":"false","ack":"false","mic":"45571d6d","appEUI":"24E124C0002A0001","fPort":"","size":"0","payloadBase64":"","payloadHex":"","enqueue":false,"classType":"Class A"},{"frequency":923300000,"power":"27","immediately":"false","dataRate":"SF10BW500","modulation":"LORA","bandwidth":500,"spreadFactor":10,"bitRate":0,"codeRate":"4/5","gatewayMac":"24E124FFFEF59D60","timeSinceGPSEpoch":"","timestamp":4104450704,"rssi":"-","loraSNR":"-","devEUI":"24E124136C379261","time":"2022-12-08T15:03:32Z","type":"DnUnc","fCnt":4,"devAddr":"060328C3","adr":"true","adrAckReq":"false","ack":"false","mic":"93917b31","appEUI":"24E124C0002A0001","fPort":"","size":"0","payloadBase64":"","payloadHex":"","enqueue":false,"classType":"Class A"},{"frequency":923300000,"power":"27","immediately":"false","dataRate":"SF10BW500","modulation":"LORA","bandwidth":500,"spreadFactor":10,"bitRate":0,"codeRate":"4/5","gatewayMac":"24E124FFFEF59D60","timeSinceGPSEpoch":"","timestamp":4085419704,"rssi":"-","loraSNR":"-","devEUI":"24E124136C379261","time":"2022-12-08T15:03:13Z","type":"DnUnc","fCnt":3,"devAddr":"060328C3","adr":"true","adrAckReq":"false","ack":"false","mic":"b925a4df","appEUI":"24E124C0002A0001","fPort":"","size":"0","payloadBase64":"","payloadHex":"","enqueue":false,"classType":"Class A"},{"frequency":923300000,"power":"27","immediately":"false","dataRate":"SF10BW500","modulation":"LORA","bandwidth":500,"spreadFactor":10,"bitRate":0,"codeRate":"4/5","gatewayMac":"24E124FFFEF59D60","timeSinceGPSEpoch":"","timestamp":4066297704,"rssi":"-","loraSNR":"-","devEUI":"24E124136C379261","time":"2022-12-08T15:02:54Z","type":"DnUnc","fCnt":2,"devAddr":"060328C3","adr":"true","adrAckReq":"false","ack":"false","mic":"2a60be57","appEUI":"24E124C0002A0001","fPort":"","size":"0","payloadBase64":"","payloadHex":"","enqueue":false,"classType":"Class A"},{"frequency":923300000,"power":"27","immediately":"false","dataRate":"SF10BW500","modulation":"LORA","bandwidth":500,"spreadFactor":10,"bitRate":0,"codeRate":"4/5","gatewayMac":"24E124FFFEF59D60","timeSinceGPSEpoch":"","timestamp":4048743704,"rssi":"-","loraSNR":"-","devEUI":"24E124136C379261","time":"2022-12-08T15:02:37Z","type":"DnUnc","fCnt":1,"devAddr":"060328C3","adr":"true","adrAckReq":"false","ack":"false","mic":"5a4018c4","appEUI":"24E124C0002A0001","fPort":"","size":"0","payloadBase64":"","payloadHex":"","enqueue":false,"classType":"Class A"},{"frequency":923300000,"power":"27","immediately":"false","dataRate":"SF10BW500","modulation":"LORA","bandwidth":500,"spreadFactor":10,"bitRate":0,"codeRate":"4/5","gatewayMac":"24E124FFFEF59D60","timeSinceGPSEpoch":"","timestamp":4030668704,"rssi":"-","loraSNR":"-","devEUI":"24E124136C379261","time":"2022-12-08T15:02:19Z","type":"DnUnc","fCnt":0,"devAddr":"060328C3","adr":"true","adrAckReq":"false","ack":"false","mic":"29f58f61","appEUI":"24E124C0002A0001","fPort":"0","size":"16","payloadBase64":"yB38MVqWRwHU8wC6sxHkTg==","payloadHex":"c81dfc315a964701d4f300bab311e44e","enqueue":false,"classType":"Class A"},{"frequency":916800000,"power":"-","immediately":"-","dataRate":"SF10BW125","modulation":"LORA","bandwidth":125,"spreadFactor":10,"bitRate":0,"codeRate":"4/5","gatewayMac":"24E124FFFEF59D60","timeSinceGPSEpoch":"376263h2m18s","timestamp":4029668704,"rssi":"-39","loraSNR":"8.0","devEUI":"24E124136C379261","time":"2022-12-08T15:02:18Z","type":"UpUnc","fCnt":1,"devAddr":"060328C3","adr":"true","adrAckReq":"false","ack":"false","mic":"686e44e3","appEUI":"24E124C0002A0001","fPort":"85","size":"6","payloadBase64":"/wv//wEB","payloadHex":"ff0bffff0101","enqueue":false,"classType":"Class A"},{"frequency":923300000,"power":"27","immediately":"false","dataRate":"SF10BW500","modulation":"LORA","bandwidth":500,"spreadFactor":10,"bitRate":0,"codeRate":"4/5","gatewayMac":"24E124FFFEF59D60","timeSinceGPSEpoch":"","timestamp":4028904704,"rssi":"-","loraSNR":"-","devEUI":"24E124136C379261","time":"2022-12-08T15:02:13Z","type":"JnAcc","fCnt":0,"devAddr":"060328C3","adr":"-","adrAckReq":"-","ack":"-","mic":"f3a2e77f","appEUI":"24E124C0002A0001","fPort":"-","size":"17","payloadBase64":"IAAAAQMCAcMoAwYIAaNgS4Y=","payloadHex":"20000001030201c32803060801a3604b86","enqueue":false,"classType":"Class A"},{"frequency":916800000,"power":"-","immediately":"-","dataRate":"SF10BW125","modulation":"LORA","bandwidth":125,"spreadFactor":10,"bitRate":0,"codeRate":"4/5","gatewayMac":"24E124FFFEF59D60","timeSinceGPSEpoch":"376263h2m13s","timestamp":4023904704,"rssi":"-41","loraSNR":"8.0","devEUI":"24E124136C379261","time":"2022-12-08T15:02:13Z","type":"JnReq","fCnt":0,"devAddr":"00000000","adr":"-","adrAckReq":"-","ack":"-","mic":"ec323904","appEUI":"24E124C0002A0001","fPort":"-","size":"18","payloadBase64":"AQAqAMAk4SRhkjdsEyThJJ7l","payloadHex":"01002a00c024e1246192376c1324e1249ee5","enqueue":false,"classType":"Class A"}],"totalCount":"10"}
```

### 3.6 下行指令发送

| 接口参数  | SDK选项            | 备注    |
| --------- | ------------------ | ------- |
| devEUI    | R/W string         | 设备EUI |
| data      | R/W string(base64) |         |
| fPort     | R/W int/string     | 端口    |
| confirmed | R/W bool           | 确认包  |

> 调用方法如下，data必须是经过base64转码的字符串

```java
JSONObject jsonObject = new JSONObject();
jsonObject.put("devEUI", urDeviceVo.getDevEUI());
jsonObject.put("fPort", fPort);
jsonObject.put("data", base64);//字符串必须先经过base64转码
jsonObject.put("confirmed", isToggleCheck2);
contentResolver.call(uri, Constant.MQTT_SEND_TX_DATA, jsonObject.toJSONString(), input);
```

### 3.7 组播指令发送

| 接口参数      | SDK选项            | 备注 |
| ------------- | ------------------ | ---- |
| multicastName | R/W string         |      |
| data          | R/W string(base64) |      |
| fPort         | R/W int/string     |      |

> 组播发送数据方法如下，注意data必须是经过base64转码的字符串

```java
JSONObject jsonObject = new JSONObject();
jsonObject.put("multicastName", vo.getName());
jsonObject.put("fPort", fPort);
String base64 = isToggleCheck ? firstInput : CommonUtil.base64Encode(firstInput, "utf-8");
jsonObject.put("data", base64);
contentResolver.call(uri, Constant.MQTT_SEND_MULTICAST_DATA, jsonObject.toJSONString(), null);
```

### 3.8 系统设置

#### 3.8.1 硬件版本

```java
contentResolver.call(uri, Constant.GET_HARDWARE, null, null);
```

#### 3.8.2 SN

```java
contentResolver.call(uri, Constant.GET_SN, null, null);
```

#### 3.8.3 软件版本

```java
contentResolver.call(uri, Constant.GET_SYSTEM_VERSION, null, null);
```

#### 3.8.4 获取USB类型

```java
contentResolver.call(uri, Constant.GET_USB_TYPE, null, null);
```

> 返回值 "true"——host模式、"false"——OTG模式

#### 3.8.5 设置USB类型

> 入参为"true"——host模式、"false"——OTG模式，无返回值

```
contentResolver.call(uri, Constant.SET_USB_TYPE, "false", null);
```



### 3.9 灯带设置

> DS7610提供灯带颜色、亮度的设置功能

```java
public static final String SET_LIGHT_RED = "set_light_red";//设置灯带红色
public static final String SET_LIGHT_GREEN = "set_light_green";//设置灯带绿色
public static final String SET_LIGHT_BLUE = "set_light_blue";//设置灯带蓝色
public static final String SET_LIGHT_WHITE = "set_light_white";//设置灯带白色
public static final String SET_BRIGHTNESS_CUSTOM = "set_brightness_custom";//设置灯带颜色，色值自定义
public static final String SET_BRIGHTNESS_1 = "set_brightness_1";//设置屏幕亮度值1
public static final String SET_BRIGHTNESS_2 = "set_brightness_2";//设置屏幕亮度值2
public static final String SET_BRIGHTNESS_3 = "set_brightness_3";//设置屏幕亮度值3
public static final String SET_BRIGHTNESS_4 = "set_brightness_4";//设置屏幕亮度值4
public static final String SET_BRIGHTNESS_CLOSE = "set_brightness_close";//关闭灯带
```

#### 3.9.1 灯带设置红色

```java
contentResolver.call(uri, Constant.SET_LIGHT_RED, null, bundle);
```

#### 3.9.2 灯带设置蓝色

```java
contentResolver.call(uri, Constant.SET_LIGHT_BLUE, null, bundle);
```

#### 3.9.3 灯带设置绿色

```java
contentResolver.call(uri, Constant.SET_LIGHT_GREEN, null, bundle);
```

#### 3.9.4 灯带设置自定义色值

```java
contentResolver.call(uri, Constant.SET_BRIGHTNESS_CUSTOM, "#EE0000", bundle);
```

>注意：入参必须是#RGB的字符串
>
>R：代表红色，值的范围（0x00-0xff），值越大红色颜色深度越深
>
>G：代表绿色，值的范围（0x00-0xff），值越大绿色颜色深度越深
>
>B：代表蓝色，值的范围（0x00-0xff），值越大蓝色颜色深度越深

#### 3.9.5 关闭灯带

```java
contentResolver.call(uri, Constant.SET_BRIGHTNESS_CLOSE, null, bundle);
```

#### 3.9.6 设置灯带亮度1

```java
contentResolver.call(uri, Constant.SET_BRIGHTNESS_1, null, bundle);
```

#### 3.9.7 设置灯带亮度2

```java
contentResolver.call(uri, Constant.SET_BRIGHTNESS_2, null, bundle);
```

#### 3.9.8 设置灯带亮度3

```java
contentResolver.call(uri, Constant.SET_BRIGHTNESS_3, null, bundle);
```



### 3.10 屏幕亮度设置

```java
public static final String GET_SCREEN_BRIGHTNESS = "get_screen_brightness";//获取屏幕亮度
public static final String SET_SCREEN_BRIGHTNESS = "set_screen_brightness";//设置屏幕亮度 屏幕亮度的范围：0 - 255，0亮度最低，255亮度最高
```

#### 3.10.1 获取屏幕亮度

> 熄屏前调用获取屏幕亮度的接口，获取到屏幕亮度，保存下来，需要亮屏的时候可以设置屏幕亮度

```java
Bundle resultBundle = contentResolver.call(uri, Constant.GET_SCREEN_BRIGHTNESS, null, bundle);
String bright = resultBundle.getString(Constant.BUNDLE_CONTENT);
```

#### 3.10.2 熄屏

>用户若是需要熄屏而不关闭屏幕，可以使用设置屏幕亮度的方法实现熄屏和亮屏功能
>
>熄屏前可以先调用获取屏幕亮度的接口获取屏幕亮度，然后设置屏幕亮度为0，亮屏时则可以设置回原先保存的屏幕亮度

```java
String brightNess = "0";
contentResolver.call(uri, Constant.SET_SCREEN_BRIGHTNESS, brightNess, bundle);
```

#### 3.10.3 亮屏

> 通过设置屏幕熄屏前的屏幕亮度实现亮屏效果

```java
contentResolver.call(uri, Constant.SET_SCREEN_BRIGHTNESS, brightNess, bundle);
```



### 3.11 获取产品型号名称

>获取产品型号名称

```java
String productname = SystemProperties.get("ro.myproduct.model");
```

