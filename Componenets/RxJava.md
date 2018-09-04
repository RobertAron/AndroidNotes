# JAVA Java

RxJava uses the observer patern more or less. It's mostly used to deal with async functions.

[Good basics on how to use RX](https://www.youtube.com/watch?v=XLH2v9deew0)

#### 1. Create an Observeable

Basically creating an iterateable the is also async.

Iterable array of numbers
```kotlin
Observable.just(1,2,3,4,5)
```
```kotlin
val names = arrayOf("bob", "bober", "bobest")

Observable.from(names)
```
<img src="https://github.com/RobertAron/AndroidNotes/blob/master/res/rxCreate.png" width="500" height="120" />

You can also `create` obersables. The three main functions in them are...

1. onNext
2. onComplete
3. onError

There is also `doOnNext` which is useful for loging things 
```kotlin
Observable.create<String> { subscriber ->
    val myString1 = veryTimeconsumingFunction()
    subscriber.onNext(myString1)
    val myString2 = otherVeryTimeconsumingFunction()
    subscriber.onNext(myString2)
    // complete stream
    subscriber.onCompelte()
}
```

Erroring can also happen.

```kotlin
Observable.create<String> { s ->
    try{
        val myString = failableFunction()
        s.onNext(myString)
    } catch (e: Error){
        s.onError(e)
    }
    s.onComplete() // <-- this will not run on error. the error will stop the stream
}
```


#### 2. Manipulate the Observable

You can merge some like `Oberserable.merge(obersable1,obersable2)`

Normal functions like `map`, `filter`,`scan`. [Use this](http://rxmarbles.com) to see a few of the functions.

#### 3. Schedulers

If you do not subscribe on your function nothing happens.

Simpliest thing you can do

```kotlin
val names = arrayOf("bob", "bober", "bobest")

Observable.from(names).subsribe{ next ->
    Log.d("MyLogTag","My data is $next")
}
```

Subscribe has 3 inputs, what to do on item, what to do on error, what to do on compelte. Probably best to log errors.

```kotlin
val names = arrayOf("bob", "bober", "bobest")

Observable.from(names).subsribe(
    { next -> Log.d("MyLogTag","My data is $next")}
    { error -> Log.d("MyLogTag","The error was $error")}
    { Log.d("MyLogTag","Finished doing all the things") }
)
```

Can unsubscribe from things

```kotlin
val names = arrayOf("bob", "bober", "bobest")

val dis: Disposable = Observable.from(names).subsribe(
    { next -> Log.d("MyLogTag","My data is $next")}
)
dis.unsubscribe()
```


##### THREADING

- `subscribeOn` is only set once, afterwards, cannot be set again. It will do nothing to call this again.
- `observeOn` will change the running thread.

# ANDROID

Make sure to include
```gradle
implementation 'io.reactivex.rxjava2:rxandroid:2.1.0'
implementation 'io.reactivex.rxjava2:rxjava:2.2.1'
```


#### Disable a button
```kotlin
myButton.setOnClickListener {
    myButton.isEnabled = false
    Observable.timer(5,TimeUnit.SECONDS)
            .subscribeOn(Schedulers.io())
            .observeOn(AndroidSchedulers.mainThread())
            .subscribe{
                myButton.isEnabled = true
                Log.d("MyTag","Button is now re-enabled")
            }
}
```


#### Basic network call

`service.fetchResult()` returns an obserable.

```kotlin
service.fetchResult()
        .subscribeOn(Schedulers.io())
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(
                { data -> process(data) },
                { error -> report_error(error) }
        )
```

#### Dispose correctly on the lifecycle.

```kotlin
override fun onPause() {
    super.onPause()
    disposable?.dispose()
}
```
