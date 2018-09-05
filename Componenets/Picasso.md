# Picaso

```gradle
dependencies {
    ...
    implementation 'com.squareup.picasso:picasso:2.71828'
    ...
}
```

```Kotlin
Picasso.get()
    .load(url)
    .resize(imageView.layoutParams.width, imageView.layoutParams.width)
//    .placeholder(R.drawable.user_placeholder)
//    .error(R.drawable.user_placeholder_error)
    .centerCrop()
    .into(imageView)
```