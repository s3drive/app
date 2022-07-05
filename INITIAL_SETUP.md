## Initial setup instructions

You need to create Backblaze B2 account: [Sign up](https://www.backblaze.com/b2/sign-up.html)

### Create bucket

[Buckets](https://secure.backblaze.com/b2_buckets.htm)

![Create bucket](https://github.com/photosync/android-sync-backblaze-b2/blob/master/resources/images/create_bucket.png?raw=true)


### Set up API key

[Application keys](https://secure.backblaze.com/app_keys.htm)

> **WARNING**: If you plan to use web client you will have to Allow access to: "All" buckets.
This is so that App have `writeCapabilities` permissions and can set up CORS Rules.


![Add Application Key](https://github.com/photosync/android-sync-backblaze-b2/blob/master/resources/images/api_key_setup.png?raw=true)


#### Copy details over to your device
![API key success prompt](https://github.com/photosync/android-sync-backblaze-b2/blob/master/resources/images/api_key_copy.png?raw=true)

> **Easy way**: You can copy&paste the whole text into: [Browser QR code generator](https://web.syncaware.com/qr) and scan it using the built-in QR scanner in phone app.
