修改android/src/main/AndroidManifest.xml  和android\app\src\profile\AndroidManifest.xml 添加下面代码  到manifest 里：



```xml
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
```

