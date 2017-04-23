---
layout: post
title:  "炬力方案系统修改杂记"
page.date:   2016-05-23 18:40:55 +0800
categories: jekyll update
---

**1，android4.4修改WiFi里显示的WiFi名**

android\frameworks\base\wifi\java\android\net\wifi\p2p目录下的WifiP2pService.java:
```java
private String getPersistedDeviceName() {
        String deviceName = Settings.Global.getString(mContext.getContentResolver(),
                Settings.Global.WIFI_P2P_DEVICE_NAME);
        if (deviceName == null) {
            /* We use the 4 digits of the ANDROID_ID to have a friendly
             * default that has low likelihood of collision with a peer */
            String id = Settings.Secure.getString(mContext.getContentResolver(),
                    Settings.Secure.ANDROID_ID);
            return "Android_" + id.substring(0,4);     //把这个return的值改成你想要的WiFi名称
        }
        return deviceName;
}
```

**2,默认位置服务选中“省电模式--Wifi定位”**

```java
 public void onRadioButtonClicked(RadioButtonPreference emiter) {
        int mode = Settings.Secure.LOCATION_MODE_OFF;
        if (emiter == mHighAccuracy) {    //默认加载的RadioButtonPreference是mHighAccuracy
//            mode = Settings.Secure.LOCATION_MODE_HIGH_ACCURACY;
              mode = Settings.Secure.LOCATION_MODE_BATTERY_SAVING;
        } else if (emiter == mBatterySaving) {
            mode = Settings.Secure.LOCATION_MODE_BATTERY_SAVING;
        } else if (emiter == mSensorsOnly) {
            mode = Settings.Secure.LOCATION_MODE_SENSORS_ONLY;
        }
        setLocationMode(mode);     //设置模式
    } 
```

**3,从文件管理器打开bmp格式的图片时，无法显示图片**

第一步： \\MLI-TCH\new_disk\GS702C_4.4\GS702C_sdk\android\packages\apps\Gallery2\src\com\android\gallery3d\app\PhotoPage.java

```java
supportedOperations ^= MediaObject.SUPPORT_EDIT;
```
修改成 

```java
// supportedOperations ^= MediaObject.SUPPORT_EDIT; 
supportedOperations &= ~MediaObject.SUPPORT_EDIT;
```

第二步：\\MLI-TCH\new_disk\GS702C_4.4\GS702C_sdk\android\packages\apps\Gallery2\src\com\android\gallery3d\data\LocalImage.java

```java
    @Override
    public int getSupportedOperations() {
        int operation = SUPPORT_DELETE | SUPPORT_SHARE | SUPPORT_CROP
                | SUPPORT_SETAS | SUPPORT_PRINT | SUPPORT_INFO;
        if (BitmapUtils.isSupportedByRegionDecoder(mimeType)) {
            operation |= SUPPORT_FULL_IMAGE | SUPPORT_EDIT;
        }
```

  修改成

```java
 @Override
    public int getSupportedOperations() {
        int operation = SUPPORT_DELETE | SUPPORT_SHARE
                | SUPPORT_SETAS | SUPPORT_PRINT | SUPPORT_INFO;
        if (BitmapUtils.isSupportedByRegionDecoder(mimeType)) {
            operation |= SUPPORT_FULL_IMAGE;
        }
        
        if (BitmapUtils.isSupportedByScaleDecoder(mimeType)) {
            operation |= SUPPORT_CROP;
            operation |= SUPPORT_EDIT;
        }
```

第三步：\\MLI-TCH\new_disk\GS702C_4.4\GS702C_sdk\android\packages\apps\Gallery2\src\com\android\gallery3d\data\UriImage.java

```java
@Override
    public int getSupportedOperations() {
        int supported = SUPPORT_PRINT | SUPPORT_SETAS;
        if (isSharable()) supported |= SUPPORT_SHARE;
        if (BitmapUtils.isSupportedByRegionDecoder(mContentType)) {
            supported |= SUPPORT_EDIT | SUPPORT_FULL_IMAGE;
        }
        return supported;
    }
```

修改成

```java
   @Override
    public int getSupportedOperations() {
        int supported = SUPPORT_PRINT | SUPPORT_SETAS;
        if (isSharable()) supported |= SUPPORT_SHARE;
        if (BitmapUtils.isSupportedByRegionDecoder(mContentType)) {
            supported |= SUPPORT_FULL_IMAGE;
        }
        
        if (BitmapUtils.isSupportedByScaleDecoder(mContentType)) {
         supported |= SUPPORT_EDIT;
        }
        
        return supported;
    }
```

第四步：

```java
    public static boolean isSupportedByRegionDecoder(String mimeType) {
        if (mimeType == null) return false;
        mimeType = mimeType.toLowerCase();
        return mimeType.startsWith("image/") && !mimeType.equals("image/gif");
        //        (!mimeType.equals("image/gif") && !mimeType.endsWith("bmp"));
    }
```

修改成

```java
     public static boolean isSupportedByRegionDecoder(String mimeType) {
        if (mimeType == null) return false;
        mimeType = mimeType.toLowerCase();
        return mimeType.startsWith("image/") && 
                (!mimeType.equals("image/gif") && !mimeType.endsWith("bmp"));
    }
    
    public static boolean isSupportedByScaleDecoder(String mimeType) {
        if (mimeType == null) return false;
        mimeType = mimeType.toLowerCase();
        return mimeType.startsWith("image/") &&
                (!mimeType.equals("image/gif"));
    }
```


