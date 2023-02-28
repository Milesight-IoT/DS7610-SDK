

English | [中文](https://github.com/Milesight-IoT/DS7610-SDK/blob/main/Docs/%E6%89%A9%E5%B1%95%E5%9D%9E%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3-v1.0.md)

# Docking Station Kit API Document

* [Docking Station Kit API Document](#docking-station-kit-api-document)
   * [1. Introduction](#1-introduction)
   * [2. Method](#2-method)
      * [2.1 Active Call](#21-active-call)
         * [2.1.1 Basic Method](#211-basic-method)
         * [2.1.2 Constant](#212-constant)
      * [2.2 Subscription](#22-subscription)
   * [3. API](#3-api)
      * [3.1 Docking Station](#31-docking-station)
         * [3.1.1 Initialization](#311-initialization)
         * [3.1.2 Cancel Initialization](#312-cancel-initialization)
         * [3.1.3 Get Connection Status Automatically](#313-get-connection-status-automatically)
         * [3.1.4 Get Connection Status Manually](#314-get-connection-status-manually)
      * [3.2 RS485](#32-rs485)
         * [3.2.1 Set Baud Rate](#321-set-baud-rate)
         * [3.2.2 Get Baud Rate](#322-get-baud-rate)
         * [3.2.3 Turn on RS485](#323-turn-on-rs485)
         * [3.2.4 Send Data](#324-send-data)
         * [3.2.5 Receive Data](#325-receive-data)
         * [3.2.6 Turn off RS485](#326-turn-off-rs485)
         * [3.2.7 Check On/Off Status](#327-check-onoff-status)
      * [3.3 DI](#33-di)
         * [3.3.1 Get DI State Automatically](#331-get-di-state-automatically)
         * [3.3.2 Get DI State Manually](#332-get-di-state-manually)
         * [3.3.3 Set DI Triggering Mode](#333-set-di-triggering-mode)
      * [3.4 DO](#34-do)
         * [3.4.1 Get DO Initial State](#341-get-do-initial-state)
         * [3.4.2 Set DO Initial State](#342-set-do-initial-state)
         * [3.4.3 Get DO State](#343-get-do-state)
         * [3.4.4 Change DO State](#344-change-do-state)

**Revision History**

| Date      | Doc Version | Description     |
| --------- | ----------- | --------------- |
| 2023.1.15 | V1.0        | Initial Version |



## 1. Introduction

This document describe APIs  of docking station kit for DS7610. G-docking station equips with below data interfaces to work with expandable applications:

- 1 × RS485
- 1 × DI (Dry Contact)
- 1 × DO (Relay Output)

For other standard APIs please refer to Android official developer guide: https://developer.android.com/guide

To develop apps for DS7610, please install Android Studio and set up your Android development environment. There is no need to install SDK and please ensure below system requirements:

- DS7610 firmware version (Builder number): 72.0.0.5-r2 and later

For more info please refer to source code of Docking Station Demo.



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
    public static final String DS_USB_RS_BROADCAST = "com.ds.system.usb.rs.broadcast";//RS485 Broadcast
    public static final String DS_USB_DI_BROADCAST = "com.ds.system.usb.di.broadcast";//DI Broadcast
    public static final String DS_USB_CONNECT_BROADCAST = "com.ds.system.usb.connect.broadcast";//USB Broadcast
    public static final String GET_USB_TYPE = "get_usb_type";//Get USB Type
    public static final String SET_USB_TYPE = "set_usb_type";//Set USB Type
    public static final String DS_USB_INIT = "ds_usb_init";//Docking station initialization
    public static final String DS_USB_UN_INIT = "ds_usb_un_init";//Cancal initialization
    public static final String DS_READ_DI_STATE = "ds_read_di_state";//Get DI state
    public static final String DS_SET_DI_AUTO_MODE = "ds_set_di_auto_mode";//Set DI Mode as Auto
    public static final String DS_SET_DI_MANUAL_MODE = "ds_set_di_manual_mode";//Set DI mode as Manual
    public static final String DS_SET_DI_INTERRUPT = "ds_set_di_interrupt";//Set DI Triggering Mode
    public static final String DS_GET_DO_INIT_TYPE = "ds_get_do_init_type";//Get Do Initial State
    public static final String DS_READ_DO_STATE = "ds_read_do_state";//Get DO State
    public static final String DS_SET_DO_INIT_STATE = "ds_set_do_init_state";//Set Do Initial State
    public static final String DS_SET_DO_STATE = "ds_set_do_state";//Change DO State
    public static final String DS_SET_BAUD_RATE = "ds_set_baud_rate";//Set RS485 Baud Rate
    public static final String DS_GET_BAUD_RATE = "ds_get_baud_rate";//Get RS485 Baud Rate
    public static final String DS_GET_USB_CONNECT_STATE = "ds_get_usb_connect_state";//Get connection status
    public static final String DS_IS_COM_OPEN = "ds_is_com_open";//Check RS485 On/off status
    public static final String DS_OPEN_COM = "ds_open_com";//Turn on RS485
    public static final String DS_CLOSE_COM = "ds_close_com";//Turn off RS485
    public static final String DS_SEND_COM_DATA = "ds_send_com_data";//Send data

    public static final String DS_DO_OPENED = "ds_do_opened";//Normally Open
    public static final String DS_DO_CLOSED = "ds_do_closed";//Normally Closed
    public static final String DS_DO_DEFAULT = "ds_do_default";//Keep the Status before Powering off

    public static final String DS_TRIG_RISING = "ds_trig_rising";//Rising Edge
    public static final String DS_TRIG_FALLING = "ds_trig_falling";//Falling Edge
    public static final String DS_TRIG_BOTH = "ds_trig_both";//Both Rising and Falling Edges
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



## 3. API

### 3.1 Docking Station

#### 3.1.1 Initialization

Before initializing the docking station, ensure the docking station is connected to DS7610 via type-C port and USB type is Host.

```
contentResolver.call(uri, Constant.DS_USB_INIT, null, null);
```

#### 3.1.2 Cancel Initialization

It's suggested to cancel the initialization of docking station when it's no longer in use.

```java
contentResolver.call(uri, Constant.DS_USB_UN_INIT, null, null);
```

#### 3.1.3 Get Connection Status Automatically

To get connection status between DS7610 and docking station automatically, register a Broadcast Receiver:

```java
connectionReceiver = new UsbConnectionReceiver();
IntentFilter intentFilter = new IntentFilter();
intentFilter.addAction(Constant.DS_USB_CONNECT_BROADCAST);
registerReceiver(connectionReceiver, intentFilter);
```

Broadcast Receiver：

```java
public class UsbConnectionReceiver extends BroadcastReceiver {

    @Override
    public void onReceive(Context context, Intent intent) {
        // TODO: This method is called when the BroadcastReceiver is receiving
        Bundle bundle = intent.getExtras();
        if (bundle != null) {
            boolean isConnected = bundle.getBoolean("message");
            EventBus.getDefault().post(new UsbConnectionEvent(isConnected));
        }
    }
}
```

#### 3.1.4 Get Connection Status Manually

```java
private boolean isUsbConnected(){
    Bundle bundle = contentResolver.call(uri, Constant.DS_GET_USB_CONNECT_STATE, null, null);
    boolean isUsbConnected = bundle.getBoolean(Constant.BUNDLE_CONTENT);
    return isUsbConnected;
}
```



### 3.2 RS485

Before turning on RS485, ensure the baud rate of docking station and RS485 terminal device are the same.

#### 3.2.1 Set Baud Rate

Supported baud rates:

```xml
<string-array name="baudrate">
    <item>1200</item>
    <item>2400</item>
    <item>4800</item>
    <item>9600</item>
    <item>19200</item>
    <item>38400</item>
    <item>57600</item>
    <item>115200</item>
</string-array>
```

```java
private boolean setBaudRate(int baudRate){
    Bundle bundle = new Bundle();
    bundle.putInt("baudRate",baudRate);
    Bundle resultBundle = contentResolver.call(uri, Constant.DS_SET_BAUD_RATE, null, bundle);
    boolean isSetSuccess = resultBundle.getBoolean(Constant.BUNDLE_CONTENT);
    return isSetSuccess;
}
```

#### 3.2.2 Get Baud Rate

**Code Example:**

```java
private int getBaudRate(Context context) {
    Bundle resultBundle = context.getContentResolver().call(uri, Constant.DS_GET_BAUD_RATE, null, null);
    int baudRate = resultBundle.getInt(Constant.BUNDLE_CONTENT);
    return baudRate;
}
```

#### 3.2.3 Turn on RS485

**Code Example:**

```java
private boolean openCOM(){
    Bundle bundle = contentResolver.call(uri, Constant.DS_OPEN_COM, null, null);
    boolean isComOpen = bundle.getBoolean(Constant.BUNDLE_CONTENT);
    return isComOpen;
}
```

#### 3.2.4 Send Data

**Code Example:**

```
private boolean sendCdcData(String msg){
    Bundle resultBundle = contentResolver.call(uri, Constant.DS_SEND_COM_DATA, msg, null);
    boolean isSetSuccess = resultBundle.getBoolean(Constant.BUNDLE_CONTENT);
    return isSetSuccess;
}
```

#### 3.2.5 Receive Data

**Code Example:**

Register a Broadcast Receiver:

```java
rsReceiver = new UsbRsReceiver();
IntentFilter intentFilter = new IntentFilter();
intentFilter.addAction(Constant.DS_USB_RS_BROADCAST);
getActivity().registerReceiver(rsReceiver, intentFilter);
```

Then you can receive the data via this Broadcast Receiver:

```java
public class UsbRsReceiver extends BroadcastReceiver {

    @Override
    public void onReceive(Context context, Intent intent) {
        // TODO: This method is called when the BroadcastReceiver is receiving
        Bundle bundle = intent.getExtras();
        if (bundle != null) {
            String data = bundle.getString("message");
            EventBus.getDefault().post(new UsbRsEvent(data));
        }
    }
}
```

#### 3.2.6 Turn off RS485

**Code Example:**

```java
private void closeCOM(){
    contentResolver.call(uri, Constant.DS_CLOSE_COM, null, null);
}
```

#### 3.2.7 Check On/Off Status

**Code Example:**

```java
private boolean isComOpen(){
    Bundle bundle = contentResolver.call(uri, Constant.DS_IS_COM_OPEN, null, null);
    boolean isComOpen = bundle.getBoolean(Constant.BUNDLE_CONTENT);
    return isComOpen;
}
```

### 3.3 DI

#### 3.3.1 Get DI State Automatically

**Code Example:**

Register a Broadcast Receiver to get DI state:

```java
usbDIReceiver = new UsbDIReceiver();
IntentFilter intentFilter = new IntentFilter();
intentFilter.addAction(Constant.DS_USB_DI_BROADCAST);
getActivity().registerReceiver(usbDIReceiver, intentFilter);
```

Broadcast Receiver：

```
public class UsbDIReceiver extends BroadcastReceiver {

    @Override
    public void onReceive(Context context, Intent intent) {
        // TODO: This method is called when the BroadcastReceiver is receiving
        Bundle bundle = intent.getExtras();
        if (bundle != null) {
            int count = bundle.getInt("message");
            EventBus.getDefault().post(new UsbDIEvent(count));
        }
    }
}
```

Set DI mode as Auto:

```java
contentResolver.call(uri, Constant.DS_SET_DI_AUTO_MODE, null, null);
```



#### 3.3.2 Get DI State Manually

If there is not need to use DI, please set the DI mode as Manual.

**Code Example:**

Set DI mode as Manual:

```java
contentResolver.call(uri, Constant.DS_SET_DI_MANUAL_MODE, null, null);
```

Get DI status manually:

```java
Bundle bundle = contentResolver.call(uri, Constant.DS_READ_DI_STATE, null, null);
String result = bundle.getString(Constant.BUNDLE_CONTENT);
```

**Return Example:**

```java
public static final String DS_LOW = "ds_low";//Low level
public static final String DS_HIGH = "ds_high";//High level
public static final String DS_NONE = "ds_none";//Fail to get DI status
```



#### 3.3.3 Set DI Triggering Mode

This only works when DI mode is Auto.

**Code Example:**

```java
contentResolver.call(uri, Constant.DS_SET_DI_INTERRUPT, Constant.DS_TRIG_BOTH, null);
```

Supported mode:

```java
public static final String DS_TRIG_RISING = "ds_trig_rising";//Rising edge
public static final String DS_TRIG_FALLING = "ds_trig_falling";//Falling edge
public static final String DS_TRIG_BOTH = "ds_trig_both";//Both rising and failling edges
```



### 3.4 DO

#### 3.4.1 Get DO Initial State

**Code Example:**

```java
Bundle bundle = contentResolver.call(uri, Constant.DS_GET_DO_INIT_TYPE, null, null);
String doInitType = bundle.getString(Constant.BUNDLE_CONTENT);
```

**Return Example:**

```java
public static final String DS_DO_OPENED = "ds_do_opened";//normally open
public static final String DS_DO_CLOSED = "ds_do_closed";//normally closed
public static final String DS_DO_DEFAULT = "ds_do_default";//keep the status before powering off
```

#### 3.4.2 Set DO Initial State

This setting will not work after docking station reboots. If you need this work rebooting, it's necessary to listen to docking station connection status broadcast and then reset the DO initial status and read DO status. For more details please refer to DoFragment.java of Docking Station Demo code.

```java
private void initDoState() {
    //Set and get DO initial state
    Bundle bundle = contentResolver.call(uri, Constant.DS_GET_DO_INIT_TYPE, null, null);
    String doInitType = bundle.getString(Constant.BUNDLE_CONTENT);
    switch (doInitType) {
        case Constant.DS_DO_OPENED:
            mSpinnerDoMode.setSelection(1);
            break;
        case Constant.DS_DO_DEFAULT:
            mSpinnerDoMode.setSelection(2);
            break;
        default:
            mSpinnerDoMode.setSelection(0);
            break;

    }
    //Get DO status
    readDoState();
}
```

**Input Parameter:**

| Name          | Property    | Description                                                  |
| ------------- | ----------- | ------------------------------------------------------------ |
| isOpen        | R/W boolean | Whether is normally open                                     |
| initStateType | R/W int     | 0: normally closed  1: normally open  2：keep the status before powering off |

#### 3.4.3 Get DO State

**Code Example:**

```java
Bundle doBundle = contentResolver.call(uri, Constant.DS_READ_DO_STATE, null, null);
String doState = doBundle.getString(Constant.BUNDLE_CONTENT);
```

**Return Example:**

```java
public static final String DS_LOW = "ds_low";//closed
public static final String DS_HIGH = "ds_high";//open
public static final String DS_NONE = "ds_none";//Fail to get DO state
```

#### 3.4.4 Change DO State

**Code Example:**

```java
Bundle bundle = new Bundle();
bundle.putBoolean("isOpen", true);
Bundle resultBundle = contentResolver.call(uri, Constant.DS_SET_DO_STATE, null, bundle);
```

| Name   | Property    | Description                                                  |
| ------ | ----------- | ------------------------------------------------------------ |
| isOpen | R/W boolean | True: set as open                                                                                                                                           False: set as closed |

