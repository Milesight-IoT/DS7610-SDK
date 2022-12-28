
English | [中文](https://github.com/Milesight-IoT/DS7610-SDK/blob/main/Docs/DS7610%20NFC%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3-v1.0.md)

[TOC]

# DS7610 NFC API Document

Table of Contents
=================

* [DS7610 NFC API Document](#ds7610-nfc-api-document)
   * [1. Introduction](#1-introduction)
   * [2. Method](#2-method)
      * [2.1 Initialization](#21-initialization)
      * [2.2 Operations](#22-operations)
         * [2.2.1 Power On Device](#221-power-on-device)
         * [2.2.2 Power Off Device](#222-power-off-device)
         * [2.2.3 Reset Device](#223-reset-device)
         * [2.2.4 Reboot Device](#224-reboot-device)
         * [2.2.5 Read Device](#225-read-device)
         * [2.2.6 Write Device](#226-write-device)
         * [2.2.7 Add Device](#227-add-device)

**Revision History**

| Date       | Doc Version | Description     |
| ---------- | ----------- | --------------- |
| 2022.12.21 | V1.0        | Initial Version |



## 1. Introduction

This document describe the API of using NFC to read and write settings of Milesight devices. For other standard NFC APIs please refer to Android official developer guide: https://developer.android.com/guide

To develop apps for DS7610 relating this feature, please deploy as below steps:

1. Install Android Studio and set up your Android development environment.
2. Add **DsNfc.arr** under directory /libs
3. Configure build.gradle file under root directory as below:

```
dependencies {
    implementation files('libs/DsNfc.aar')
}
```



## 2. Method

### 2.1 Initialization

There is only need to initialize once and it is suggested to initialize it on main interface or Application page.

```java
	@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        DsNfcManager.getInstance().config("123456", 4);
    }
```

Please configure the Activity of NFC action on Manifest file:

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

Add below settings to ensure the feature of reading/writing Milesight devices take precedence over other NFC programs:

```java
public abstract class BaseNFCActivity extends AppCompatActivity {

    private PendingIntent mPendingIntent;
    private NfcAdapter mNfcAdapter;

    @Override
    protected void onStart() {
        super.onStart();
        mNfcAdapter = NfcAdapter.getDefaultAdapter(this);
        mPendingIntent = PendingIntent.getActivity(this, 0, new Intent(this, getClass()), 0);
    }

    @Override
    public void onResume() {
        super.onResume();
        //Take precedence over other NFC processes
        if (mNfcAdapter != null)
            mNfcAdapter.enableForegroundDispatch(this, mPendingIntent, null, null);
    }

    @Override
    public void onPause() {
        super.onPause();
        //Restore to default
        if (mNfcAdapter != null)
            mNfcAdapter.disableForegroundDispatch(this);
    }
}
```



### 2.2 Operations

Please refer the way to get mifareUltralight as below:

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

For more details please refer to LoRaWAN Gateway Demo source code.



#### 2.2.1 Power On Device

```java
DsNfcManager.getInstance().powerOn(mifareUltralight);
```

#### 2.2.2 Power Off Device

```java
DsNfcManager.getInstance().powerOff(mifareUltralight);
```

#### 2.2.3 Reset Device

```java
DsNfcManager.getInstance().reset(mifareUltralight);
```

#### 2.2.4 Reboot Device

```java
DsNfcManager.getInstance().restart(mifareUltralight);
```

#### 2.2.5 Read Device

```java
nfcResult = DsNfcManager.getInstance().readFlashData(mifareUltralight, position -> fragment.setProgress(position));
```

#### 2.2.6 Write Device

```java
nfcResult = DsNfcManager.getInstance().writeFlashData(mifareUltralight, lastBytes, JSON.toJSONString(modifyMap), position -> fragment.setProgress(position));
```

#### 2.2.7 Add Device

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
