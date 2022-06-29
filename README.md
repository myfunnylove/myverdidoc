# verdi scan app
## Для идентификации через Verdi

Для идентфикации нужен сдк файл:
`myverdi-auth-release.aar`

Переместим СДК в папку `libs`

Добавим его к приложению:
```gradle
    implementation files('libs/myverdi-auth-release.aar')
```

Дополнительные настройки:

settings.gradle
```gradle
    maven {
            url "https://maven.regulaforensics.com/RegulaDocumentReader"
          }

```
build.gradle (App)
```gradle
 implementation ('com.regula.documentreader:api:6.4.7224@aar'){
        transitive = true
    }

  implementation 'com.regula.documentreader.core:fullrfid:6.4.7188@aar'
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
            appHashIdForSmsAutoFill = "2BFC3U+OIBJ", // hash code for autofill otp

            styleRes = R.style.Theme_SampleApp, // apptheme
            )
            .setDrawableLogo(R.drawable.flash) // some logo
            .setPrimaryColor(Color.parseColor("#FFBB86FC")) // optional primaryColor
            .build()
    }
}
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


### для автопостановлении ОТП код:
```kotlin
val smsCode = //полученный смс код
MyVerdi.get().getOtpListener()?.invoke(Otp(smsCode ?: ""))

```
