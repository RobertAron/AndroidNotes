### build.gradle (Module:app)
```gradle
dependencies {
    ...
    implementation 'com.android.support:design:27.1.1'
    ...
}
```

This will add features like `tools:showIn="navigation_view"` to xml.

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
