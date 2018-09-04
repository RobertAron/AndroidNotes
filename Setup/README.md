### Design Tools

This will add features like `tools:showIn="navigation_view"` to xml.

`build.gradle (Module: app)`

```gradle
dependencies {
    ...
    implementation 'com.android.support:design:27.1.1'
    
    implementation 'io.reactivex.rxjava2:rxandroid:2.1.0'
    implementation 'io.reactivex.rxjava2:rxjava:2.2.1'
    
    implementation "com.squareup.retrofit2:retrofit:2.4.0"
    implementation "com.squareup.retrofit2:converter-gson:2.3.0"
    implementation "com.squareup.retrofit2:adapter-rxjava2:2.3.0"
    ...
}
```

### Drawer navigation.

`values/strings.xml`

```xml
<resources>
    ...
    <string name="navigation_drawer_open">Open navigation drawer</string>
    <string name="navigation_drawer_close">Close navigation drawer</string>
    ...
</resources>
```

### Change to Material Setup

Remove the action bar.

```xml
<resources>
    <!-- Base application theme. -->
    <style name="AppTheme.Base" parent="Theme.AppCompat.Light.NoActionBar">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">#3F51B5</item>
        <!-- Light Indigo -->
        <item name="colorPrimaryDark">#3949AB</item>
        <!-- Dark Indigo -->
        <item name="colorAccent">#00B0FF</item>
        <!-- Blue -->
    </style>
    <style name="AppTheme" parent="AppTheme.Base"></style>
</resources>
```

### Change Colors.

<img src="https://github.com/RobertAron/AndroidNotes/blob/master/res/color-notes.png" width="200" height="400" />
