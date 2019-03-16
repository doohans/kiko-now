---
layout: post
title: "Android Bluetooth Low Energy 샘플 동작 불가"
tags: [android,ble, bluetooth]
comments: true
published : true
---

Android 6.0 Marshmallow 이상에서 BLE 사용하기

### google BLE 예제 동작 불가

아두이노로 BLE 관련 things를 만들려고 한다. BLE 모듈은 HM-10의 클론을 HM-10 정펌 올린 것이다.
아래 google의 BLE 예제를 clone 받아서 실행보아도 전혀 scan이 되지 않았다. 어플리케이션 구동은 되지만 scan이 되지 않는다. 

* https://github.com/googlesamples/android-BluetoothLeGatt

다른 저자의 몇가지 샘플을 실행해 보아도 마찬가지이다. 혹시나 싶어 구글 플레이의 BLE Scanner를 내려 받아보니 정상 검색이 된다.

scan 버튼을 눌렀을 때 로그는 아래와 같다.

```
2019-03-16 23:00:09.586 18956-18988/com.example.android.bluetoothlegatt W/Binder: Caught a RuntimeException from the binder stub implementation.
    java.lang.SecurityException: Need ACCESS_COARSE_LOCATION or ACCESS_FINE_LOCATION permission to get scan results
        at android.os.Parcel.createException(Parcel.java:1950)
        at android.os.Parcel.readException(Parcel.java:1918)
        at android.os.Parcel.readException(Parcel.java:1868)
        at android.bluetooth.IBluetoothGatt$Stub$Proxy.startScan(IBluetoothGatt.java:1004)
        at android.bluetooth.le.BluetoothLeScanner$BleScanCallbackWrapper.onScannerRegistered(BluetoothLeScanner.java:460)
        at android.bluetooth.le.IScannerCallback$Stub.onTransact(IScannerCallback.java:57)
        at android.os.Binder.execTransact(Binder.java:731)
```        
*ACCESS_COARSE_LOCATION* 에 관련된 문제가 보인다. 


### Android 6.0 Marshmallow 보안 정책 변경

* https://developer.radiusnetworks.com/2015/09/29/is-your-beacon-app-ready-for-android-6.html

>The biggest change for beacon apps in Android 6.0, codenamed Marshmallow, and sometimes called just “M”, has to do with permissions. Just like iOS, Android now implements permissions at runtime instead of the traditional way of granting permissions at install time. Apps designed for Marshmallow (SDK 23 and above) must add code to prompt users for some permissions after the app starts up, otherwise they will not be granted.

BLE 사용시에 Runtime Permmission이 생겼단다. 링크된 경로에 보면 전력관련 글도 보이니 상세한 내용은 해당 링크를 참조한다.

문제를 해결하기 위해서는 Runtime Permmission을 수락할 수 있도록 다음의 코드가 추가되어야 한다.

AndroidManifest.xml root에 아래의 퍼미션을 추가해야 한다.

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION”/>
```
그리고 첫 Activity에 다음의 코드를 삽입하여 퍼미션을 사용자에게 득해야 한다. 아래 코드는 alert 창이 추가 되었지만 없어도 무방하다. android 기본 수락 창이 뜬다.

```java
private static final int PERMISSION_REQUEST_COARSE_LOCATION = 1;
// ...

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    // ...

    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) { 
        // Android M Permission check 
        if (this.checkSelfPermission(Manifest.permission.ACCESS_COARSE_LOCATION) != PackageManager.PERMISSION_GRANTED) { 
            final AlertDialog.Builder builder = new AlertDialog.Builder(this); 
            builder.setTitle("This app needs location access");
            builder.setMessage("Please grant location access so this app can detect beacons.");
            builder.setPositiveButton(android.R.string.ok, null); 
            builder.setOnDismissListener(new DialogInterface.OnDismissListener() {  
                @Override 
                public void onDismiss(DialogInterface dialog) {
                    requestPermissions(new String[]{Manifest.permission.ACCESS_COARSE_LOCATION}, PERMISSION_REQUEST_COARSE_LOCATION); 
                }  
            }); 
            builder.show(); 
        }
    }
}

@Override
public void onRequestPermissionsResult(int requestCode,
                                       String permissions[],
                                       int[] grantResults) {
    switch (requestCode) {
        case PERMISSION_REQUEST_COARSE_LOCATION: {
            if (grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                Log.d(TAG, "coarse location permission granted");
            } else {
                final AlertDialog.Builder builder = new AlertDialog.Builder(this);
                builder.setTitle("Functionality limited");
                builder.setMessage("Since location access has not been granted, this app will not be able to discover beacons when in the background.");
                builder.setPositiveButton(android.R.string.ok, null);
                builder.setOnDismissListener(new DialogInterface.OnDismissListener() {
                    @Override
                    public void onDismiss(DialogInterface dialog) {
                    }
                });
                builder.show();
            }
            return;
        }
    }
}
```


