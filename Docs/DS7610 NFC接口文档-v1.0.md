
[English](https://github.com/Milesight-IoT/DS7610-SDK/edit/main/Docs/DS7610%20NFC%20API%20Document-V1.0.md) | 中文

# DS7610 NFC接口文档

* [DS7610 NFC接口文档](#ds7610-nfc接口文档)
   * [1.前提条件](#1前提条件)
      * [1.1 部署Android开发环境](#11-部署android开发环境)
      * [1.2 导入项目依赖](#12-导入项目依赖)
   * [2. 实现流程](#2-实现流程)
      * [2.1 初始化](#21-初始化)
      * [2.2 操作节点设备](#22-操作节点设备)
         * [2.2.1节点设备开机](#221节点设备开机)
         * [2.2.2节点设备关机](#222节点设备关机)
         * [2.2.3节点设备重置](#223节点设备重置)
         * [2.2.4节点设备重启](#224节点设备重启)
         * [2.2.5读节点设备信息](#225读节点设备信息)
         * [2.2.6写入节点设备信息](#226写入节点设备信息)
         * [2.2.7一键添加节点设备](#227一键添加节点设备)

<center><big>文档修订记录</big></center>

| 版本 | 变更时间  | 备注 |
| ---- | --------- | ---- |
| V1.0 | 2022.12.1 | 初稿 |



## 1.前提条件

### 1.1 部署Android开发环境

下载安装Android studio，配置好开发环境。

**注意：本接口文档是配合DS7610使用的且仅用于NFC配置星纵智能NFC设备，其它NFC功能实现请直接参考Android标准接口：https://developer.android.com/guide****

### **1.2 导入项目依赖**

在项目的libs目录下添加DsNfc.aar包

在项目根目录build.gradle文件里配置 

```
dependencies {
    implementation files('libs/DsNfc.aar')
}
```



## 2. 实现流程

### 2.1 初始化

初始化只需一次，建议在Application或者主页面初始化

```java
	@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        DsNfcManager.getInstance().config("123456", 4);
    }
```

操作NFC的Activity在Manifest需要做如下配置

```xml
		<activity
            android:name=".ui.nfc.NfcActivity">
            <intent-filter>
                <action android:name="android.nfc.action.TECH_DISCOVERED" />
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>

            <meta-data
                android:name="android.nfc.action.TECH_DISCOVERED"
                android:resource="@xml/nfc_tech_filter" />
        </activity>
```

为了NFC能够优于其他NFC程序处理必须做如下设置

```java
public abstract class BaseNFCActivity extends AppCompatActivity {

    private PendingIntent mPendingIntent;
    private NfcAdapter mNfcAdapter;

    @Override
    protected void onStart() {
        super.onStart();
        //此处adapter需要重新获取，否则无法获取message
        mNfcAdapter = NfcAdapter.getDefaultAdapter(this);
        //一旦截获NFC消息，就会通过PendingIntent调用窗口
        mPendingIntent = PendingIntent.getActivity(this, 0, new Intent(this, getClass()), 0);
    }

    @Override
    public void onResume() {
        super.onResume();
        //设置处理优于所有其他NFC的处理
        if (mNfcAdapter != null)
            mNfcAdapter.enableForegroundDispatch(this, mPendingIntent, null, null);
    }

    @Override
    public void onPause() {
        super.onPause();
        //恢复默认状态
        if (mNfcAdapter != null)
            mNfcAdapter.disableForegroundDispatch(this);
    }
}
```



### 2.2 操作节点设备

可以参考LoRaWanGatewayDemo中NfcActivity的使用方法，其中mifareUltralight获取方式如下：

```java
@Override
public void onNewIntent(Intent intent) {
    super.onNewIntent(intent);
    operateMifareUltralight(intent, opType);
}

public void operateMifareUltralight(Intent intent, int type) {
    if (intent != null) {
        Tag tag = intent.getParcelableExtra(NfcAdapter.EXTRA_TAG);
        if (tag == null) return;
        String[] techList = tag.getTechList();
        if (Arrays.toString(techList).contains("MifareUltralight") && task == null) {
            showNFCDialog(null, false);
            task = new AsyncTask<Void, Void, NfcResult>() {
                @Override
                protected NfcResult doInBackground(Void... voids) {
                    MifareUltralight mifareUltralight = MifareUltralight.get(tag);
                    ...
                }
                ...
            };
            task.execute();

        }
    }
}
```



#### 2.2.1节点设备开机

```java
DsNfcManager.getInstance().powerOn(mifareUltralight);
```

#### 2.2.2节点设备关机

```java
DsNfcManager.getInstance().powerOff(mifareUltralight);
```

#### 2.2.3节点设备重置

```java
DsNfcManager.getInstance().reset(mifareUltralight);
```

#### 2.2.4节点设备重启

```java
DsNfcManager.getInstance().restart(mifareUltralight);
```

#### 2.2.5读节点设备信息

```java
nfcResult = DsNfcManager.getInstance().readFlashData(mifareUltralight, position -> fragment.setProgress(position));
```

#### 2.2.6写入节点设备信息

```java
nfcResult = DsNfcManager.getInstance().writeFlashData(mifareUltralight, lastBytes, JSON.toJSONString(modifyMap), position -> fragment.setProgress(position));
```

#### 2.2.7一键添加节点设备

```java
nfcResult = DsNfcManager.getInstance().oneKeyAddDsDevice(mifareUltralight, profile, nfcInfo, position -> {
    fragment.setProgress(position);
});
if (nfcResult.getCode() == NfcResult.SUCCESS) {
    Bundle bundle = getContentResolver().call(uri, Constant.UR_DEVICES_ADD, nfcResult.getJson(), null);
    int code = bundle.getInt(Constant.BUNDLE_CODE, -1);
    String json = bundle.getString(Constant.BUNDLE_CONTENT);
    nfcResult.setCode(code);
    nfcResult.setJson(json);
}
```

其中profile和nfcInfo的获取参考LoRaWanGatewayDemo中的NfcActivity



