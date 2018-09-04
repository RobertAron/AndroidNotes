# Retrofit

This assumes knowledge of RxJava.

#### 1. Import stuff.

```gradle
implementation "com.squareup.retrofit2:retrofit:2.4.0"
compile "com.squareup.retrofit2:converter-gson:2.3.0"
compile "com.squareup.retrofit2:adapter-rxjava2:2.3.0"
```

#### 2. Specify the API call return type.

The actual variable name will be pulled from the data. Careful that each one is actually named correctly.

```kotlin
data class ReturnString(val id:String)
```


#### 3. Create the client.

This part uses annontations to generate classes later. Different annotations can create different queries.

```kotlin
interface GitHubClient {
    @GET("/users/{user}/repos")
    fun reposForUser(@Path("user") user:String):
            Observable<List<ReturnString>>
}
```

#### 4. Build the API

- `.addCallAdapterFactory(RxJava2CallAdapterFactory.create())` function changes the output of our retrofit into an Observable.
- `.addConverterFactory(GsonConverterFactory.create())` specifies the output type which in this case is json.

```kotlin
val myAPI = Retrofit
        .Builder()
        .baseUrl("https://api.github.com")
        .addCallAdapterFactory(RxJava2CallAdapterFactory.create())
        .addConverterFactory(GsonConverterFactory.create())
        .build()
        .create(GitHubClient::class.java)
```

#### 5. Access the Obserable of a call to process data.

```kotlin
val disposable = myAPI.reposForUser("RobertAron")
        .subscribe(
            {result -> result.map {println(it)}},
            {error -> println(error)}
        )
```

#### 6. Make sure to dispose of things you don't want anymore.

In android....

```kotlin
override fun onPause() {
    super.onPause()
    disposable?.dispose()
}
```