# 안드로이드(PERMISSION)

### permission(권한)

* 안드로이드는 마시멜로우 버전부터 권한을부여하는 방식이 바뀜 
* 이전 버전에는 앱을 설치할 때 건한을 사용한닥고 사용자에게 말하기 때문에 많은사용자들이 마무 생각없이 설치하는 경우가 많았음
* 이에 따라 설치된 앱의 주요기능을 마음대로 사용할 수 있었다.
* 이는 보안적으로 많은 문제를 발생
* 이러한 이유 때문에 마시멜로우 버전 이상 부터는 앱을 실행시에 권한을 부여하도록 변경



## permission (권한) 종류

### 1. 일반권한



### 2.위험권한







### 오류 확인(웹)





![11](C:\Users\student\Desktop\11.PNG)



manifest xml 설정

* net:::ERR_CACHE_MISS : 권한 부여 

  ```java
  <uses-permission android:name="com.exam.permission.JAVA_PERMISSION" />
  ```

  

* net:::ERR_ACCESS_DENIED

  



```java
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="multi.android.datamanagementpro">

    <uses-permission android:name="com.exam.permission.JAVA_PERMISSION" />
    <uses-permission android:name="android.permission.INTERNET"/>
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme"
        android:usesCleartextTraffic="true"> 
        <activity android:name=".Permission.UseCustomPermission"></activity>
        <activity android:name=".Permission.BasicPermissionTest">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```