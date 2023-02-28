

English | [中文](https://github.com/Milesight-IoT/DS7610-SDK/blob/main/Docs/DS7610%C2%A0LoRaWAN%E5%92%8C%E7%B3%BB%E7%BB%9F%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3-v1.1.md)

# DS7610 LoRaWAN&System API Document

* [DS7610 LoRaWAN&amp;System API Document](#ds7610-lorawansystem-api-document)
   * [1. Introduction](#1-introduction)
   * [2. Method](#2-method)
      * [2.1 Active Call](#21-active-call)
         * [2.1.1 Basic Method](#211-basic-method)
         * [2.1.2 Constant](#212-constant)
      * [2.2 Subscription](#22-subscription)
   * [3. API](#3-api)
      * [3.1 LoRaWAN Gateway](#31-lorawan-gateway)
         * [3.1.1 Enable Gateway](#311-enable-gateway)
         * [3.1.2 Disable Gateway](#312-disable-gateway)
         * [3.1.3 Get Enable State](#313-get-enable-state)
         * [3.1.4 Get Running State](#314-get-running-state)
         * [3.1.5 Get Gateway Configuration](#315-get-gateway-configuration)
         * [3.1.6 Get LoRaWAN Frequency List](#316-get-lorawan-frequency-list)
         * [3.1.7 Get LoRaWAN Datarate List](#317-get-lorawan-datarate-list)
         * [3.1.8 Set Gateway Configuration](#318-set-gateway-configuration)
      * [3.2 Profile](#32-profile)
         * [3.2.1 Get Profile](#321-get-profile)
         * [3.2.2 Add Profile](#322-add-profile)
         * [3.2.3 Modify Profile](#323-modify-profile)
         * [3.2.4 Delete profile](#324-delete-profile)
      * [3.3 Multicast](#33-multicast)
         * [3.3.1 Introduction](#331-introduction)
         * [3.3.2 Add Multicast Group](#332-add-multicast-group)
         * [3.3.3 Get Multicast Group](#333-get-multicast-group)
         * [3.3.4 Modify Multicast Group](#334-modify-multicast-group)
         * [3.3.5 Delete Multicast Group](#335-delete-multicast-group)
      * [3.4 Device](#34-device)
         * [3.4.1 Get Device](#341-get-device)
         * [3.4.2 Add Device](#342-add-device)
            * [3.4.2.1 Add by Device EUI](#3421-add-by-device-eui)
            * [3.4.2.2 Add by Multiple Parameters](#3422-add-by-multiple-parameters)
            * [3.4.2.3 Add by NFC](#3423-add-by-nfc)
         * [3.4.3 Modify Device](#343-modify-device)
         * [3.4.4 Delete Device](#344-delete-device)
      * [3.5 Get Packet](#35-get-packet)
      * [3.6 Send Downlink](#36-send-downlink)
      * [3.7 Send Multicast Downlink](#37-send-multicast-downlink)
      * [3.8 System Settings](#38-system-settings)
         * [3.8.1 Get Hardware Version](#381-get-hardware-version)
         * [3.8.2 Get Serial Number](#382-get-serial-number)
         * [3.8.3 Get Firmware Version](#383-get-firmware-version)
         * [3.8.4 Get USB Type](#384-get-usb-type)
         * [3.8.5 Set USB Type](#385-set-usb-type)
      * [3.9 Light Strip Setting](#39-light-strip-setting)
         * [3.9.1 Set Light Strip as Red](#391-set-light-strip-as-red)
         * [3.9.2 Set Light Strip as Blue](#392-set-light-strip-as-blue)
         * [3.9.3 Set Light Strip as Green](#393-set-light-strip-as-green)
         * [3.9.4 Set Light Strip as Custom Color](#394-set-light-strip-as-custom-color)
         * [3.9.5 Turn off Light Strip](#395-turn-off-light-strip)
         * [3.9.6 Set Light Level 1](#396-set-light-level-1)
         * [3.9.7 Set Light Level 2](#397-set-light-level-2)
         * [3.9.8 Set Light Level 3](#398-set-light-level-3)
      * [3.10 Screen Light Settings](#310-screen-light-settings)
         * [3.10.1 Get Screen Light Level](#3101-get-screen-light-level)
         * [3.10.2 Screen Dimmed](#3102-screen-dimmed)
         * [3.10.3 Screen On](#3103-screen-on)
      * [3.11 Get Model Name](#311-get-model-name)

**Revision History**

| Date       | Doc Version | Description                                                  |
| ---------- | ----------- | ------------------------------------------------------------ |
| 2022.12.19 | V1.0        | Initial Version                                              |
| 2023.1.31  | V1.1        | Add light strip settings, screen light settings and obtain model name |



## 1. Introduction

This document describe below APIs  of DS7610:

- LoRaWAN Gateway
- System

For other standard APIs please refer to Android official developer guide: https://developer.android.com/guide

To develop apps for DS7610, please install Android Studio and set up your Android development environment. There is no need to install SDK and please ensure below system requirements:

- DS7610 firmware version (Builder number): 72.0.0.5-r2 and later



## 2. Method

### 2.1 Active Call

#### 2.1.1 Basic Method

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

#### 2.1.2 Constant

```java
public class Constant {
    public static final String BUNDLE_CODE = "code";//Return code
    public static final String BUNDLE_CONTENT = "content";//Return content
    public static final String DS_SYSTEM_URI = "content://com.ds.system.provider";//provider's URI
    public static final String SUBSCRIBE_ACTION = "com.ds.system.mqtt.broadcast";//subscribe action
    
    public static final String ENABLE_LORA_NS = "enable_lora_ns";//Enable LoRaWAN gateway
    public static final String DISABLE_LORA_NS = "disable_lora_ns";//Disable LoRaWAN gateway
    public static final String GET_LORA_NS_STATE = "get_lora_ns_state";//Get gateway enable status
    public static final String GET_LORA_NS_RUN_STATE = "get_lora_ns_run_state";//Get gateway runnging status
    public static final String LORA_CONFIG_QUERY_SERVER_NAME = "lora_config_query_server_name";//Get gateway configuration
    public static final String LORA_CONFIG_QUERY_FREQUENCY = "lora_config_query_frequency";//Get frequency list
    public static final String LORA_CONFIG_QUERY_DATA_RATES = "lora_config_query_data_rates";//Get datarate list
    public static final String LORA_CONFIG_UPDATE = "lora_config_update";//Set gateway
    public static final String GET_LORA_DATA_WITH_NFC = "get_lora_data_with_nfc";//Get necessary gateway info to add device via NFC
    
    public static final String DS_PROFILES_QUERY = "ds_profiles_query";//Get profile
    public static final String DS_PROFILES_ADD = "ds_profiles_add";//Add profile
    public static final String DS_PROFILES_UPDATE = "ds_profiles_update";//Modify profile
    public static final String DS_PROFILES_DELETE = "ds_profiles_delete";//Delete profile
    
    public static final String DS_MULTICAST_GROUPS_ADD = "ds_multicast_groups_add";//Add multicast group
    public static final String DS_MULTICAST_GROUPS_QUERY = "ds_multicast_groups_query";//Get multicast group
    public static final String DS_MULTICAST_GROUPS_UPDATE = "ds_multicast_groups_update";//Modify multicast group
    public static final String DS_MULTICAST_GROUPS_DELETE = "ds_multicast_groups_delete";//Delete multicast group
    
    public static final String DS_DEVICES_QUERY = "ds_devices_query";//Get device
    public static final String DS_DEVICES_ADD = "ds_devices_add";//Add device
    public static final String DS_DEVICES_UPDATE = "ds_devices_update";//Modify device
    public static final String DS_DEVICES_DELETE = "ds_devices_delete";//Delete device
    
    public static final String DS_PACKETS_QUERY = "ds_packets_query";//Get uplink and downlink packets
 
    public static final String MQTT_SEND_TX_DATA = "send_tx_data";//Send downlink
    
    public static final String MQTT_SEND_MULTICAST_DATA = "send_multicast_data";//Send multicast downlink
    
    public static final String GET_HARDWARE = "get_hardware";//Get hardware version
    public static final String GET_SN = "get_sn";//sn
    public static final String GET_SYSTEM_VERSION = "get_system_version";//Get firmware version
    public static final String GET_USB_TYPE = "get_usb_type";//Get USB type
    public static final String SET_USB_TYPE = "set_usb_type";//Set USB type
}
```

### 2.2 Subscription

Besides active call, you can also register Broadcast Receiver to subscribe device information on main interface or Application as below:

```java
myReceiver = new MyReceiver();
IntentFilter intentFilter = new IntentFilter();
intentFilter.addAction(Constant.SUBSCRIBE_ACTION);
registerReceiver(myReceiver, intentFilter);
```

Broadcast Receiver：

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

Subscribe topics:

```java
contentResolver.call(uri, Constant.MQTT_SUBSCRIBE_RX, urDeviceVo.getDevEUI(), null);
```

Optional topics：

```java
public static final String MQTT_SUBSCRIBE_ALL = "mqtt_subscribe_all";        //Subscribe all except multicast
public static final String MQTT_UNSUBSCRIBE_ALL = "mqtt_unsubscribe_all";    //Unsubscribe all except multicast
public static final String MQTT_SUBSCRIBE_JOIN = "mqtt_subscribe_join";      //Subscribe join notification
public static final String MQTT_UNSUBSCRIBE_JOIN = "mqtt_unsubscribe_join";  //Unsubscribe join notification
public static final String MQTT_SUBSCRIBE_RX = "mqtt_subscribe_rx";          //Subscribe uplink data
public static final String MQTT_UNSUBSCRIBE_RX = "mqtt_unsubscribe_rx";      //Unsubscribe uplink data
public static final String MQTT_SUBSCRIBE_ACK = "mqtt_subscribe_ack";        //Subscribe ACK notification
public static final String MQTT_UNSUBSCRIBE_ACK = "mqtt_unsubscribe_ack";    //Unsubscribe ACK notification
public static final String MQTT_SUBSCRIBE_ERROR = "mqtt_subscribe_error";    //Subscribe Error notification
public static final String MQTT_UNSUBSCRIBE_ERROR = "mqtt_unsubscribe_error";//Unsubscribe Error notification
public static final String MQTT_SUBSCRIBE_TX = "mqtt_subscribe_tx";          //Subscribe downlink data
public static final String MQTT_UNSUBSCRIBE_TX = "mqtt_unsubscribe_tx";      //Unsubscribe downlink data
```

**Return Example:**

```
topic=application/1/device/24e124136c379261/rx, message={"applicationID":"1","applicationName":"app","deviceName":"24e124136c379261","devEUI":"24e124136c379261","rxInfo":[{"mac":"24e124fffef59d60","time":"2022-12-08T15:02:18Z","rssi":-39,"loRaSNR":8,"name":"Local Gateway","latitude":0,"longitude":0,"altitude":0}],"txInfo":{"frequency":916800000,"dataRate":{"modulation":"LORA","bandwidth":125,"spreadFactor":10},"adr":true,"codeRate":"4/5"},"fCnt":1,"fPort":85,"data":"/wv//wEB","time":"2022-12-08T15:02:18Z"}
```



## 3. API

### 3.1 LoRaWAN Gateway

DS7610 can work as a single channel LoRaWAN gateway to receive LoRaWAN data from Milesight devices. Now it's fixed as embeded network server mode.

#### 3.1.1 Enable Gateway

**Code Example:**

```java
public void setLoraNsEnable() {
    resolver.call(uri, ENABLE_LORA_NS, null, null);
}
```

#### 3.1.2 Disable Gateway

**Code Example:**

```java
public void setLoraNsDisable() {
    resolver.call(uri, DISABLE_LORA_NS, null, null);
}
```

#### 3.1.3 Get Enable State

**Code Example:**

```java
public boolean isLoraNsEnable() {
    Bundle bundle = resolver.call(uri, GET_LORA_NS_STATE, null, null);
    int code = bundle.getInt(BUNDLE_CODE, -1);
    boolean isEnable = LORA_STATE_ENABLE == code;
    return isEnable;
}
```

#### 3.1.4 Get Running State

**Code Example:**

```java
public int getLoraRunState() {
    Bundle bundle = resolver.call(uri, GET_LORA_NS_RUN_STATE, null, null);
    int code = bundle.getInt(BUNDLE_CODE, -1);
    return code;
}
```

**Returns:** 

```java
public static final int LORA_STATE_CLOSED = -103; //Gateway Service Close
public static final int LORA_STATE_FAIL = -104;   //Gateway Service open/close failed
public static final int LORA_STATE_OPENED = -105; //Gateway Service Open
```

#### 3.1.5 Get Gateway Configuration

**Code Example:**

```java
Bundle input = new Bundle();
input.putString("servName", servNameVo.getServName());
Bundle bundle = resolver.call(uri, Constant.LORA_CONFIG_QUERY_SERVER_NAME, null, input);
```

**Return Example:**

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

| Name          | Property   | Description                                    |
| ------------- | ---------- | ---------------------------------------------- |
| gatewayID     | R string   | Gateway EUI, read-only                         |
| servName      | R/W string | Frequency Channel Plan                         |
| rxFrequence   | R/W int    | Frequency                                      |
| rxDataRate    | R/W int    | Datarate (DR)                                  |
| servNames     | R string   | Get optional frequency channel plan, read-only |
| serverAddress | R/W string | It's fixed as embedded network server          |

#### 3.1.6 Get LoRaWAN Frequency List

**Code Example:**

```java
Bundle input = new Bundle();
input.putString("servName", servNameVo.getServName());
Bundle bundle = resolver.call(uri, Constant.LORA_CONFIG_QUERY_FREQUENCY, null, input);
```

**Return Example:**

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

| Name             | Property | Description        |
| :--------------- | -------- | ------------------ |
| channel          | int      | LoRa channel index |
| frequency        | string   | Frequency          |
| minFrequency     | string   | Min frequency      |
| maxFrequency     | string   | Max frequency      |
| defaultFrequency | string   | Default frequency  |

#### 3.1.7 Get LoRaWAN Datarate List

**Code Example:**

```java
Bundle input = new Bundle();
input.putString("servName", servNameVo.getServName());
input.putString("rxFrequence", servNameVo.getRxFrequence());
Bundle bundle = resolver.call(uri, Constant.LORA_CONFIG_QUERY_DATA_RATES, null, input);
```

**Return Example:**

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

| Name         | Property | Description      |
| ------------ | -------- | ---------------- |
| datarate     | int      | Datarate(DR)     |
| spreadfactor | int      | Spreading Factor |
| defultDr     | int      | Default datarate |

#### 3.1.8 Set Gateway Configuration

**Code Example:**

```java
Bundle updateBundle = resolver.call(uri, Constant.LORA_CONFIG_UPDATE, JSON.toJSONString(servNameVo), null);
int updateCode = updateBundle.getInt(Constant.BUNDLE_CODE, -1);
```

**Input Parameter Example:**

```java
{
    "gatewayID": "24E124FFFEF59F32",
    "rxFrequence": 868100000,
    "rxDataRate": 4,
    "servName": "EU868"
}
```

| Name        | Property   | Description            |
| ----------- | ---------- | ---------------------- |
| gatewayID   | R string   | Gateway EUI, read-only |
| servName    | R/W string | Channel plan           |
| rxFrequence | R/W int    | Frequency              |
| rxDataRate  | R/W int    | Datarate (DR)          |



### 3.2 Profile

A device profile defines the device capabilities and boot parametrs which shall be provided by LoRaWAN end-device manufacturer. It includes below parameters:

| Name           | Property   | Description                      |
| -------------- | ---------- | -------------------------------- |
| name           | R/W string | Device profile name              |
| rxDataRate2    | R/W int    | Device RX2 data rate             |
| rxFreq2        | R/W int    | Device RX2 frequency             |
| supportsJoin   | R/W bool   | Join type, true:OTAA   false:ABP |
| supportsClassB | R/W bool   | Whether work mode is Class B     |
| supportsClassC | R/W bool   | Whether work mode is Class C     |

DS7610 has already defined 8 device files which are **non-editable** and **non-deletable**, and the rxDataRate2 and rxFreq2 will update automatically according to channel plan. Please refer to default returns of chapter 3.2.1.

Default and optional rxDataRate2 value:

| Channel Plan | **CN470**       | **EU868**       | **IN865**       | **RU864**       | **KR920**       | **US915**        | **AU915**        | **AS923-1**     | **AS923-2**     | **AS923-3**     | **AS923-4**     |
| ------------ | --------------- | --------------- | --------------- | --------------- | --------------- | ---------------- | ---------------- | --------------- | --------------- | --------------- | --------------- |
| **Default**  | DR0 (SF12,125k) | DR0 (SF12,125k) | DR2 (SF10,125k) | DR0 (SF12,125k) | DR0 (SF12,125k) | DR8 (SF12,500k)  | DR8 (SF12,500k)  | DR2 (SF10,125k) | DR2 (SF10,125k) | DR2 (SF10,125k) | DR2 (SF10,125k) |
| **Optional** | DR0 (SF12,125k) | DR0 (SF12,125k) | DR0 (SF12,125k) | DR0 (SF12,125k) | DR0 (SF12,125k) | DR8 (SF12,500k)  | DR8 (SF12,500k)  | DR0 (SF12,125k) | DR0 (SF12,125k) | DR0 (SF12,125k) | DR0 (SF12,125k) |
|              | DR1 (SF11,125k) | DR1 (SF11,125k) | DR1 (SF11,125k) | DR1 (SF11,125k) | DR1 (SF11,125k) | DR9 (SF11,500k)  | DR9 (SF11,500k)  | DR1 (SF11,125k) | DR1 (SF11,125k) | DR1 (SF11,125k) | DR1 (SF11,125k) |
|              | DR2 (SF10,125k) | DR2 (SF10,125k) | DR2 (SF10,125k) | DR2 (SF10,125k) | DR2 (SF10,125k) | DR10 (SF10,500k) | DR10 (SF10,500k) | DR2 (SF10,125k) | DR2 (SF10,125k) | DR2 (SF10,125k) | DR2 (SF10,125k) |
|              | DR3 (SF9,125k)  | DR3 (SF9,125k)  | DR3 (SF9,125k)  | DR3 (SF9,125k)  | DR3 (SF9,125k)  | DR11 (SF9,500k)  | DR11 (SF9,500k)  | DR3 (SF9,125k)  | DR3 (SF9,125k)  | DR3 (SF9,125k)  | DR3 (SF9,125k)  |
|              | DR4 (SF8,125k)  | DR4 (SF8,125k)  | DR4 (SF8,125k)  | DR4 (SF8,125k)  | DR4 (SF8,125k)  | DR12 (SF8,500k)  | DR12 (SF8,500k)  | DR4 (SF8,125k)  | DR4 (SF8,125k)  | DR4 (SF8,125k)  | DR4 (SF8,125k)  |
|              | DR5 (SF7,125k)  | DR5 (SF7,125k)  | DR5 (SF7,125k)  | DR5 (SF7,125k)  | DR5 (SF7,125k)  | DR13 (SF7,500k)  | DR13 (SF7,500k)  | DR5 (SF7,125k)  | DR5 (SF7,125k)  | DR5 (SF7,125k)  | DR5 (SF7,125k)  |

Default and optional rxFreq2 value: 

| Channel Plan             | **CN470**   | **EU868** | **IN865** | **RU864** | **KR920**   | **US915**       | **AU915**       | **AS923-1** | **AS923-2** | **AS923-3** | **AS923-4** |
| ------------------------ | ----------- | --------- | --------- | --------- | ----------- | --------------- | --------------- | ----------- | ----------- | ----------- | ----------- |
| **Default（MHz)**        | 505.3       | 869.525   | 866.55    | 869.1     | 921.9       | 923.3           | 923.3           | 923.2       | 921.4       | 916.6       | 917.3       |
| **Optional Range（MHz)** | 500.3~509.7 | 863~870   | 865~867   | 864~870   | 920.9~923.3 | **923.3~927.5** | **923.3~927.5** | 915~928     | 915~928     | 915~928     | 915~928     |



#### 3.2.1 Get Profile

**Code Example:**

```java
Bundle bundle = contentResolver.call(uri, Constant.DS_PROFILES_QUERY, null, null);
String json = bundle.getString(Constant.BUNDLE_CONTENT);
int code = bundle.getInt(Constant.BUNDLE_CODE, -1);
```

**Default Returns:**

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

#### 3.2.2 Add Profile

**Code Example:**

```java
Bundle bundle = contentResolver.call(uri, Constant.DS_PROFILES_ADD, text, null);
String json = bundle.getString(Constant.BUNDLE_CONTENT);
```

**Input Parameter Example:**

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

**Return Example：**

```json
{"profileID":"2725d684-7404-4a68-9520-790d366a990b"}
```

#### 3.2.3 Modify Profile

**Code Example:** 

```java
Bundle input = new Bundle();
input.putString("factor", name); //modify profile name as factor
Bundle bundle = contentResolver.call(uri, Constant.DS_PROFILES_UPDATE, content, input);
int code = bundle.getInt(Constant.BUNDLE_CODE, -1);
String json = bundle.getString(Constant.BUNDLE_CONTENT);
```

**Input Parameter Example:**

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

**Note:** default 8 device profiles are non editable.

#### 3.2.4 Delete profile

**Code Example:**

```java
Bundle input = new Bundle();
input.putString("factor", name);
Bundle bundle = contentResolver.call(uri, Constant.DS_PROFILES_DELETE, "", input);
int code = bundle.getInt(Constant.BUNDLE_CODE, -1);
String json = bundle.getString(Constant.BUNDLE_CONTENT);
```

**Note:** default 8 device profiles are non delatable.



### 3.3 Multicast

#### 3.3.1 Introduction

DS7610 supports sending downlink commands to a groups of devices by multicast feature or Milesight D2D feature.

**Multicast:** DS7610 can send a single downlink payload to a group of devices via LoRaWAN multicast feature. All devices in this group share the same multicast address, multicast network session keys and multicast application session keys. This only works with Class C devices which supports multicast feature.

**Milesight D2D:**  Milesight D2D protocol is used for setting up transmission among Milesight devices directly. DS7610 can work as Milesight D2D controller to send a single downlink payload to a group of Milesight D2D agent devices. All devices share the same D2D key. DS7610 has already defined a Milesight D2D multicast which is **non-editable** and **non-deletable**, and the frequency and dr of this group will update automatically according to channel plan. 

| Name      | Property          | Description                                                  |
| --------- | ----------------- | ------------------------------------------------------------ |
| name      | R/W string        | Multicast name                                               |
| id        | R string          | Multicast ID, geneate automatically after creating           |
| mcAddr    | R/W string(HEX8)  | Multicast address                                            |
| mcKey     | R/W string(HEX32) | Milesight D2D key                                            |
| mcNwkSKey | R/W string(HEX32) | Multicast network session key                                |
| mcAppSKey | R/W string(HEX32) | Multicast application session key                            |
| frequency | R/W int           | Freuency to send multicast downlinks, 0 means default frequency and dr |
| dr        | R/W int           | Datarate to send multicast downlinks                         |

#### 3.3.2 Add Multicast Group

**Add Multicast Group Example:**

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

**Add Milesight D2D Group Example:**

```java
JSONObject jsonObject = new JSONObject();
jsonObject.put("name", firstInput);
jsonObject.put("mcKey", secondInput);
jsonObject.put("frequency", Integer.valueOf(frequency));
jsonObject.put("dr", Integer.valueOf(dr));
Bundle bundle = contentResolver.call(uri, Constant.DS_MULTICAST_GROUPS_ADD, jsonObject.toJSONString(), null);
```

#### 3.3.3 Get Multicast Group

**Code Example:**

```java
Bundle bundle = contentResolver.call(uri, Constant.DS_MULTICAST_GROUPS_QUERY, null, null);
int code = bundle.getInt(Constant.BUNDLE_CODE, -1);
String jsonStr = bundle.getString(Constant.BUNDLE_CONTENT);
```

**Return Example：**

```
{
    "totalCount":2,
    "result":[
        {
            "id":"8eccb9b6-6c46-449c-881c-bc9ce0ec7ca0",
            "name":"D2D",  //Default D2D Multicast
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

#### 3.3.4 Modify Multicast Group

**Code Example:**

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

**Note:** default D2D multicast is non editable.

#### 3.3.5 Delete Multicast Group

**Code Example:**

```java
Bundle input = new Bundle();
input.putString("factor", vo.getName());
Bundle bundle = contentResolver.call(uri, Constant.DS_MULTICAST_GROUPS_DELETE, "", input);
```

**Note:** default D2D multicast is non deletable.



### 3.4 Device

DS7610 only supports connecting to Milesight LoRaWAN end devices. Before connecting, ensure the single channel mode of end device is enabled. It includes below parameters:

| Parameter    | Property          | Description                                                  |
| ------------ | ----------------- | ------------------------------------------------------------ |
| name         | R/W string        | Device name                                                  |
| devEUI       | R/W string(HEX16) | Device EUI, after creating it can't be modified              |
| profileName  | R/W string        | Device profile name                                          |
| appKey       | R/W string(HEX32) | App key, it can be modified when join type is OTAA           |
| devAddr      | R/W string(HEX8)  | Device address, it can be modified when join type is ABP     |
| appSKey      | R/W string(HEX32) | AppSKey, it can be modified when join type is ABP            |
| nwkSKey      | R/W string(HEX32) | NwkSKey, it can be modified when join type is ABP            |
| fCntUp       | R/W int/string    | Uplink frame-counter, it can be modified when join type is ABP |
| fCntDown     | R/W int/string    | Downlink frame-counter, it can be modified when join type is ABP |
| active       | R bool            | Network join status                                          |
| supportsJoin | R bool            | Join type, true:OTAA, false:ABP                              |

#### 3.4.1 Get Device

**Get All Devices Example:**

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

**Search for one device by device EUI:**

```java
Bundle input = new Bundle();
input.putString("factor", urDeviceVo.getDevEUI());
bundle = contentResolver.call(uri, Constant.DS_DEVICES_SEARCH, "", input);
String json = bundle.getString(Constant.BUNDLE_CONTENT);
int code = bundle.getInt(Constant.BUNDLE_CODE, -1);
```

**Return Example：**

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



#### 3.4.2 Add Device

##### 3.4.2.1 Add by Device EUI

**Code Example:**

```java
Map<String, Object> map = new HashMap<>();
map.put("name", firstInput);
map.put("devEUI", secondInput);
map.put("appKey", defaultKey);
Bundle bundle = contentResolver.call(uri, Constant.DS_DEVICES_ADD, JSON.toJSONString(map), null);
```

##### 3.4.2.2 Add by Multiple Parameters

**Code Example:**

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

##### 3.4.2.3 Add by NFC

DS7610 has equipped NFC to add Milesight LoRaWAN end devices with NFC feature easily. If the channel plan between DS7610 and LoRaWAN devices are different, DS7610 can modify the parameters of devices via NFC to match the settings and add the devices again. For details please refer to LoRa Gateway Demo code.

**Code Example:**

```java
Bundle nfcBundle = getContentResolver().call(uri, Constant.GET_LORA_DATA_WITH_NFC, null, null);//Return necessary gateway parameters
int nfcCode = nfcBundle.getInt(Constant.BUNDLE_CODE, -1);
String nfcInfo = nfcBundle.getString(Constant.BUNDLE_CONTENT);
Bundle profileBundle = getContentResolver().call(uri, Constant.DS_PROFILES_QUERY, null, null);//Return device profile list
int profileCode = profileBundle.getInt(Constant.BUNDLE_CODE, -1);
String profile = profileBundle.getString(Constant.BUNDLE_CONTENT);
if (profileCode == 0 && nfcCode == 0) {
    //Add device
    nfcResult = DsNfcManager.getInstance().oneKeyAddDsDevice(mifareUltralight, profile, nfcInfo, position -> {
        fragment.setProgress(position);
    });
    if (nfcResult.getCode() == NfcResult.SUCCESS) {
        //Add device
        Bundle bundle = getContentResolver().call(uri, Constant.DS_DEVICES_ADD, nfcResult.getJson(), null);
        ...
    }
}
```



#### 3.4.3 Modify Device

**Code Example:**

```java
private void save() {
    ...
    map.put("name", name);
    map.put("description", name);
    map.put("devEUI", devEUI);
    map.put("devAddr", devAddr);
    map.put("supportsJoin", selectProfile.isSupportsJoin());
    //Select according to join type of device profile
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



#### 3.4.4 Delete Device

**Code Example:**

```java
Bundle input = new Bundle();
input.putString("factor", urDeviceVo.getDevEUI());
Bundle bundle = contentResolver.call(uri, Constant.DS_DEVICES_DELETE, "", input);
int code = bundle.getInt(Constant.BUNDLE_CODE, -1);
if (code == 0) {
    //Success
} else {
    //Fail
}
```



### 3.5 Get Packet

DS7610 supports checking at most 500 uplink and downlink packets details.

**Code Example:** Get latest 10 packets' details

```
Bundle input = new Bundle();
input.putInt("offset", 0);//start query number, 0 means the latest packet
input.putInt("limit", 10);//Packet amount
Bundle bundle = contentResolver.call(uri, Constant.DS_PACKETS_QUERY, null, input);
```

**Return Example：**

```json
{"packets":[{"frequency":923300000,"power":"27","immediately":"false","dataRate":"SF10BW500","modulation":"LORA","bandwidth":500,"spreadFactor":10,"bitRate":0,"codeRate":"4/5","gatewayMac":"24E124FFFEF59D60","timeSinceGPSEpoch":"","timestamp":4141465704,"rssi":"-","loraSNR":"-","devEUI":"24E124136C379261","time":"2022-12-08T15:04:09Z","type":"DnUnc","fCnt":6,"devAddr":"060328C3","adr":"true","adrAckReq":"false","ack":"false","mic":"c84067ad","appEUI":"24E124C0002A0001","fPort":"","size":"0","payloadBase64":"","payloadHex":"","enqueue":false,"classType":"Class A"},{"frequency":923300000,"power":"27","immediately":"false","dataRate":"SF10BW500","modulation":"LORA","bandwidth":500,"spreadFactor":10,"bitRate":0,"codeRate":"4/5","gatewayMac":"24E124FFFEF59D60","timeSinceGPSEpoch":"","timestamp":4123548704,"rssi":"-","loraSNR":"-","devEUI":"24E124136C379261","time":"2022-12-08T15:03:52Z","type":"DnUnc","fCnt":5,"devAddr":"060328C3","adr":"true","adrAckReq":"false","ack":"false","mic":"45571d6d","appEUI":"24E124C0002A0001","fPort":"","size":"0","payloadBase64":"","payloadHex":"","enqueue":false,"classType":"Class A"},{"frequency":923300000,"power":"27","immediately":"false","dataRate":"SF10BW500","modulation":"LORA","bandwidth":500,"spreadFactor":10,"bitRate":0,"codeRate":"4/5","gatewayMac":"24E124FFFEF59D60","timeSinceGPSEpoch":"","timestamp":4104450704,"rssi":"-","loraSNR":"-","devEUI":"24E124136C379261","time":"2022-12-08T15:03:32Z","type":"DnUnc","fCnt":4,"devAddr":"060328C3","adr":"true","adrAckReq":"false","ack":"false","mic":"93917b31","appEUI":"24E124C0002A0001","fPort":"","size":"0","payloadBase64":"","payloadHex":"","enqueue":false,"classType":"Class A"},{"frequency":923300000,"power":"27","immediately":"false","dataRate":"SF10BW500","modulation":"LORA","bandwidth":500,"spreadFactor":10,"bitRate":0,"codeRate":"4/5","gatewayMac":"24E124FFFEF59D60","timeSinceGPSEpoch":"","timestamp":4085419704,"rssi":"-","loraSNR":"-","devEUI":"24E124136C379261","time":"2022-12-08T15:03:13Z","type":"DnUnc","fCnt":3,"devAddr":"060328C3","adr":"true","adrAckReq":"false","ack":"false","mic":"b925a4df","appEUI":"24E124C0002A0001","fPort":"","size":"0","payloadBase64":"","payloadHex":"","enqueue":false,"classType":"Class A"},{"frequency":923300000,"power":"27","immediately":"false","dataRate":"SF10BW500","modulation":"LORA","bandwidth":500,"spreadFactor":10,"bitRate":0,"codeRate":"4/5","gatewayMac":"24E124FFFEF59D60","timeSinceGPSEpoch":"","timestamp":4066297704,"rssi":"-","loraSNR":"-","devEUI":"24E124136C379261","time":"2022-12-08T15:02:54Z","type":"DnUnc","fCnt":2,"devAddr":"060328C3","adr":"true","adrAckReq":"false","ack":"false","mic":"2a60be57","appEUI":"24E124C0002A0001","fPort":"","size":"0","payloadBase64":"","payloadHex":"","enqueue":false,"classType":"Class A"},{"frequency":923300000,"power":"27","immediately":"false","dataRate":"SF10BW500","modulation":"LORA","bandwidth":500,"spreadFactor":10,"bitRate":0,"codeRate":"4/5","gatewayMac":"24E124FFFEF59D60","timeSinceGPSEpoch":"","timestamp":4048743704,"rssi":"-","loraSNR":"-","devEUI":"24E124136C379261","time":"2022-12-08T15:02:37Z","type":"DnUnc","fCnt":1,"devAddr":"060328C3","adr":"true","adrAckReq":"false","ack":"false","mic":"5a4018c4","appEUI":"24E124C0002A0001","fPort":"","size":"0","payloadBase64":"","payloadHex":"","enqueue":false,"classType":"Class A"},{"frequency":923300000,"power":"27","immediately":"false","dataRate":"SF10BW500","modulation":"LORA","bandwidth":500,"spreadFactor":10,"bitRate":0,"codeRate":"4/5","gatewayMac":"24E124FFFEF59D60","timeSinceGPSEpoch":"","timestamp":4030668704,"rssi":"-","loraSNR":"-","devEUI":"24E124136C379261","time":"2022-12-08T15:02:19Z","type":"DnUnc","fCnt":0,"devAddr":"060328C3","adr":"true","adrAckReq":"false","ack":"false","mic":"29f58f61","appEUI":"24E124C0002A0001","fPort":"0","size":"16","payloadBase64":"yB38MVqWRwHU8wC6sxHkTg==","payloadHex":"c81dfc315a964701d4f300bab311e44e","enqueue":false,"classType":"Class A"},
{"frequency":916800000,"power":"-","immediately":"-","dataRate":"SF10BW125","modulation":"LORA","bandwidth":125,"spreadFactor":10,"bitRate":0,"codeRate":"4/5","gatewayMac":"24E124FFFEF59D60","timeSinceGPSEpoch":"376263h2m18s","timestamp":4029668704,"rssi":"-39","loraSNR":"8.0","devEUI":"24E124136C379261","time":"2022-12-08T15:02:18Z","type":"UpUnc","fCnt":1,"devAddr":"060328C3","adr":"true","adrAckReq":"false","ack":"false","mic":"686e44e3","appEUI":"24E124C0002A0001","fPort":"85","size":"6","payloadBase64":"/wv//wEB","payloadHex":"ff0bffff0101","enqueue":false,"classType":"Class A"},{"frequency":923300000,"power":"27","immediately":"false","dataRate":"SF10BW500","modulation":"LORA","bandwidth":500,"spreadFactor":10,"bitRate":0,"codeRate":"4/5","gatewayMac":"24E124FFFEF59D60","timeSinceGPSEpoch":"","timestamp":4028904704,"rssi":"-","loraSNR":"-","devEUI":"24E124136C379261","time":"2022-12-08T15:02:13Z","type":"JnAcc","fCnt":0,"devAddr":"060328C3","adr":"-","adrAckReq":"-","ack":"-","mic":"f3a2e77f","appEUI":"24E124C0002A0001","fPort":"-","size":"17","payloadBase64":"IAAAAQMCAcMoAwYIAaNgS4Y=","payloadHex":"20000001030201c32803060801a3604b86","enqueue":false,"classType":"Class A"},
{"frequency":916800000,"power":"-","immediately":"-","dataRate":"SF10BW125","modulation":"LORA","bandwidth":125,"spreadFactor":10,"bitRate":0,"codeRate":"4/5","gatewayMac":"24E124FFFEF59D60","timeSinceGPSEpoch":"376263h2m13s","timestamp":4023904704,"rssi":"-41","loraSNR":"8.0","devEUI":"24E124136C379261","time":"2022-12-08T15:02:13Z","type":"JnReq","fCnt":0,"devAddr":"00000000","adr":"-","adrAckReq":"-","ack":"-","mic":"ec323904","appEUI":"24E124C0002A0001","fPort":"-","size":"18","payloadBase64":"AQAqAMAk4SRhkjdsEyThJJ7l","payloadHex":"01002a00c024e1246192376c1324e1249ee5","enqueue":false,"classType":"Class A"}],"totalCount":"10"}
```



| Name         | Property          | Description                                                  |
| ------------ | ----------------- | ------------------------------------------------------------ |
| type         | R string          | **JnAcc:** Join Accept from NS<br/>**JnReq:** Join Request from LoRaWAN nodes<br/>**UpUnc:** Unconfirmed uplink<br/>**UpCnf:** Confirmed uplink<br/>**DnUnc:** Unconfirmed downlink<br/>**DnCnf:** Confirmed downlink |
| payloadHex   | R string(HEX)     | Payload content, Hex format                                  |
| fPort        | R int/string      | Application port                                             |
| fCnt         | R int/string      | Frame counter                                                |
| time         | R string          | It's fixed as GMT time                                       |
| rssi/loraSNR | R string(int/int) | Signal value                                                 |



### 3.6 Send Downlink

**Code Example:**

```java
JSONObject jsonObject = new JSONObject();
jsonObject.put("devEUI", urDeviceVo.getDevEUI());
jsonObject.put("fPort", fPort);
jsonObject.put("data", base64);
jsonObject.put("confirmed", isToggleCheck2);
contentResolver.call(uri, Constant.MQTT_SEND_TX_DATA, jsonObject.toJSONString(), input);
```

| Name      | Property           | Description                                                |
| --------- | ------------------ | ---------------------------------------------------------- |
| devEUI    | R/W string         | Device EUI                                                 |
| data      | R/W string(base64) | Downlink command, base64 format string                     |
| fPort     | R/W int/string     | Application port, it's 85 by default for Mileisght devices |
| confirmed | R/W bool           | Confirmed or unconfirmed                                   |



### 3.7 Send Multicast Downlink

**Code Example:**

```java
JSONObject jsonObject = new JSONObject();
jsonObject.put("multicastName", vo.getName());
jsonObject.put("fPort", fPort);
String base64 = isToggleCheck ? firstInput : CommonUtil.base64Encode(firstInput, "utf-8");
jsonObject.put("data", base64);
contentResolver.call(uri, Constant.MQTT_SEND_MULTICAST_DATA, jsonObject.toJSONString(), null);
```

| Name          | Property           | Description                                                |
| ------------- | ------------------ | ---------------------------------------------------------- |
| multicastName | R/W string         | Multicast name                                             |
| data          | R/W string(base64) | Downlink command, base64 format string                     |
| fPort         | R/W int/string     | Application port, it's 85 by default for Mileisght devices |



### 3.8 System Settings

#### 3.8.1 Get Hardware Version

```java
contentResolver.call(uri, Constant.GET_HARDWARE, null, null);
```



#### 3.8.2 Get Serial Number

```java
contentResolver.call(uri, Constant.GET_SN, null, null);
```



#### 3.8.3 Get Firmware Version

```java
contentResolver.call(uri, Constant.GET_SYSTEM_VERSION, null, null);
```



#### 3.8.4 Get USB Type

```java
contentResolver.call(uri, Constant.GET_USB_TYPE, null, null);
```

**Returns:**

"true"——Host

"false"——OTG



#### 3.8.5 Set USB Type

```
contentResolver.call(uri, Constant.SET_USB_TYPE, "false", null);
```

**Input Parameters:**

"true"——Host

"false"——OTG



### 3.9 Light Strip Setting

DS7610 provides API to configure the color and light level of front and back light strips.

```java
public static final String SET_LIGHT_RED = "set_light_red";//Set light strip as red
public static final String SET_LIGHT_GREEN = "set_light_green";//Set light strip as green
public static final String SET_LIGHT_BLUE = "set_light_blue";//Set light strip as blue
public static final String SET_LIGHT_WHITE = "set_light_white";//Set light strip as white
public static final String SET_BRIGHTNESS_CUSTOM = "set_brightness_custom";//Set light strip as custom color
public static final String SET_BRIGHTNESS_1 = "set_brightness_1";//Set light light level 1
public static final String SET_BRIGHTNESS_2 = "set_brightness_2";//Set light light level 2
public static final String SET_BRIGHTNESS_3 = "set_brightness_3";//Set light light level 3
public static final String SET_BRIGHTNESS_4 = "set_brightness_4";//Set light light level 4
public static final String SET_BRIGHTNESS_CLOSE = "set_brightness_close";//Turn off light strip
```

#### 3.9.1 Set Light Strip as Red

```java
contentResolver.call(uri, Constant.SET_LIGHT_RED, null, bundle);
```



#### 3.9.2 Set Light Strip as Blue

```java
contentResolver.call(uri, Constant.SET_LIGHT_BLUE, null, bundle);
```



#### 3.9.3 Set Light Strip as Green

```java
contentResolver.call(uri, Constant.SET_LIGHT_GREEN, null, bundle);
```



#### 3.9.4 Set Light Strip as Custom Color

The input parameter should be RGB strings.

```java
contentResolver.call(uri, Constant.SET_BRIGHTNESS_CUSTOM, "#EE0000", bundle);
```



#### 3.9.5 Turn off Light Strip

```java
contentResolver.call(uri, Constant.SET_BRIGHTNESS_CLOSE, null, bundle);
```



#### 3.9.6 Set Light Level 1

```java
contentResolver.call(uri, Constant.SET_BRIGHTNESS_1, null, bundle);
```



#### 3.9.7 Set Light Level 2

```java
contentResolver.call(uri, Constant.SET_BRIGHTNESS_2, null, bundle);
```



#### 3.9.8 Set Light Level 3

```java
contentResolver.call(uri, Constant.SET_BRIGHTNESS_3, null, bundle);
```



### 3.10 Screen Light Settings

Users can get the screen light level when screen is on, then turn on or off the screen by adjusting the light level.

```java
public static final String GET_SCREEN_BRIGHTNESS = "get_screen_brightness";//Get screen light level
public static final String SET_SCREEN_BRIGHTNESS = "set_screen_brightness";//Set screen light level as 0 to 255
```



#### 3.10.1 Get Screen Light Level

Get the light level of screen before it dimmed.

```java
Bundle resultBundle = contentResolver.call(uri, Constant.GET_SCREEN_BRIGHTNESS, null, bundle);
String bright = resultBundle.getString(Constant.BUNDLE_CONTENT);
```

#### 3.10.2 Screen Dimmed

Set the light level of screen as 0.

```java
String brightNess = "0";
contentResolver.call(uri, Constant.SET_SCREEN_BRIGHTNESS, brightNess, bundle);
```

#### 3.10.3 Screen On

Get the light level before screen off and set this light level to achieve the screen on.

```java
contentResolver.call(uri, Constant.SET_SCREEN_BRIGHTNESS, brightNess, bundle);
```



### 3.11 Get Model Name

Get the device model name.

```java
String productname = SystemProperties.get("ro.myproduct.model");
```

### 

