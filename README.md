# Kotlin GoogleApiClient with RxKotlin
Easy handling of GoogleApiClient with RxKotlin

# Usage
```Kotlin
class TestClass(val context: Context) {

    companion object {
        val TAG = this::class.java.simpleName
    }

    private val googleApiHelperRx: GoogleApiHelperRx = GoogleApiHelperRx()

    fun doSomethingWithGoogleApi() {

        // Define ConnectionCallback which be called after GoogleApiClient connected successfully
        val connCallback = object : GoogleApiHelperRx.GoogleApiClientCallbackRx {
            override fun onConnected(client: GoogleApiClient, emitter: ObservableEmitter<GoogleApiClient>) {
                // Use emitter to control the flow
                Log.w(TAG, "do something cool")
                emitter.onComplete()

                // User client to disconnect from GoogleApiClient
                // client.disconnect()
            }

        }

        googleApiHelperRx.build(context, connCallback)
                .subscribeOn(Schedulers.io())
                .observeOn(AndroidSchedulers.mainThread())
                .doOnComplete {
                    Log.w(TAG, "onComplete")
                }
                .doOnError {
                    Log.w(TAG, "onError" + it.message)
                }
                .subscribe()
    }
}
```
