# MyVerdi SDK
## Для идентификации через MyVerdi

Для идентфикации нужен сдк:

Добавим его к приложению:
```groovy
    implementation 'com.github.myfunnylove:myverdisdk:1.0.3'
```

Дополнительные настройки:

settings.gradle
```groovy
    maven { url 'https://jitpack.io' }
    maven {
            url "https://maven.regulaforensics.com/RegulaDocumentReader"
    }

```


AndroidManifest.xml
```xml
    <uses-permission android:name="android.permission.NFC" />
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.CAMERA"/>
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>

    <uses-feature android:name="android.hardware.camera" />
    <uses-feature android:name="android.hardware.camera.autofocus" />
    <uses-feature android:name="android.hardware.camera.flash" android:required="false" />
    <uses-feature android:name="android.hardware.screen.landscape"/>

        <activity
            android:name="com.verdi.TuneVerdiScanActivity"
            android:screenOrientation="portrait"
            android:exported="false"/>
```

## Пример использование СДК сперва нужно инициализация СДК:
```kotlin
class App : Application() {

    override fun onCreate() {
        super.onCreate()


        MyVerdi.Builder(
            mContext = this,
            appId = "BHHZ1Lk8MOiVSyug", // client App ID тестовый "BHHZ1Lk8MOiVSyug"
            appHashIdForSmsAutoFill = "2BFC3U+OIBJ", // hash code для автозаполнение СМС

            styleRes = R.style.Theme_SampleApp, // Тема приложении
            )
            .setDrawableLogo(R.drawable.flash) // Логотир
            .setPrimaryColor(Color.parseColor("#FFBB86FC")) // primaryColor
            .build()
    }
}
```



### для автопостановлении ОТП код:
```kotlin
val smsCode = //полученный смс код
MyVerdi.get().getOtpListener()?.invoke(Otp(smsCode ?: ""))

```

### Для регистрации через MyVerdi:

```kotlin
MyVerdi.get()
        .startRegistration(this@MainActivity, successListener = {
                        guid ->

                    }, failListener = {
                        error ->

                    })
```

### Для верификации через MyVerdi:

```kotlin
MyVerdi.get()
       .startVerification(this@MainActivity, successListener = {
                                guid ->


                        }, failListener = {
                                error ->
                            

                        })
```


