# Deep Link, Web Link, App Link

## Deep link
Deep links are URIs of any scheme that take users directly to a specific part of your app. To create deep links, add intent filters to drive users to the right activity in your app, as shown in the following code snippet:
```kotlin
<activity
    android:name=".MyMapActivity"
    android:exported="true"
    ...>
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="geo" />
    </intent-filter>
</activity>
```
When the user clicks a deep link, a disambiguation dialog might appear. This dialog allows the user to select one of multiple apps, including your app, that can handle the given deep link.

In other app:
```kotlin
binding.btn.setOnClickListener {
    val intent = Intent (Intent.ACTION_VIEW, "https://hehe".toUri())
    intent.putExtra("deep_link", "XIN CHAO, Ban vua mo deep link")
    startActivity(intent)
}
```
![img.png](The%20disambiguation%20dialog.png)

## Web link
Web links are deep links that use the HTTP and HTTPS schemes. On Android 12 and higher, clicking a web link (that is not an Android App Link) always shows content in a web browser.

On devices running previous versions of Android, if your app or other apps installed on a user's device can also handle the web link, users might not go directly to the browser. Instead, they'll see a disambiguation dialog similar to the one that appears in figure 2.

The following code snippet shows an example of a web link filter:
```kotlin
<intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="android.intent.category.BROWSABLE" />

    <data android:scheme="http" />
    <data android:host="myownpersonaldomain.com" />
</intent-filter>
```

## Android App Links
Android App Links, available on Android 6.0 (API level 23) and higher, are web links that use the HTTP and HTTPS schemes and contain the autoVerify attribute. This attribute allows your app to designate itself as the default handler of a given type of link. So when the user clicks on an Android App Link, your app opens immediately if it's installed—the disambiguation dialog doesn't appear.

If the user doesn't want your app to be the default handler, they can override this behavior from the app's settings.

```kotlin
<intent-filter android:autoVerify="true">
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="android.intent.category.BROWSABLE" />

    <!-- Do not include other schemes. -->
    <data android:scheme="http" />
    <data android:scheme="https" />

    <data android:host="myownpersonaldomain.com" />
</intent-filter>
```

### Add Android App Links
The general steps for creating Android App Links are as follows:
1. **Create deep links to specific content in your app**: In your app manifest, create intent filters for your website URIs and configure your app to use data from the intents to send users to the right content in your app. 
2. **Add verification for your deep links**: Configure your app to request verification of app links. Then, publish a Digital Asset Links JSON file on your websites to verify ownership through Google Search Console. 

