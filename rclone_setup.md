
| NOTE: This is Contributor's Content and not part of S3Drive official documentation. Please use at your own risk. We accept improvements via [Pull Requests](https://github.com/s3drive/app/pulls) |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|


## Setting up Rclone encryption with S3Drive and MinIO back-end

## Prerequisites
This guide assumes that you have `Rclone` already installed on your system and `MinIO` endpoint already available. In this guide we've used `Ubuntu` OS, commands on `Windows` might slightly differ.

### Rclone
Please refer to relevant install instructions:
https://rclone.org/downloads/

| Use at least the `1.63` version in order to use filename encryption and S3Drive disk mount |
|--------------------------------------------------------------------------------------------|


### MinIO
https://min.io/download#/docker

## Rclone configuration


| WARNING: Rclone is actively maintained and below used selection numbers might change in the future versions. Always double check what option number you select. |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|

Open Rclone config creator:
`rclone config`

Upon finish, Rclone will create a config file in the location depending on the OS.
Type: `rclone config file` to display file location.

Type `n` for a new remote, then give it a name. In this example we call it `minio`

Next we need to choose which backend shall be used. Since MinIO is Amazon S3 compliant storage, we type `4`.
It is recommended to use back-end native protocol. For instance for Backblaze their native API (option `6`) is more efficient than S3.

![rclone1](https://raw.githubusercontent.com/s3drive/app/master/resources/images/rclone/rclone1.png)

From the S3 back-end selection list, choose MinIO. (Option `7` in our case)

In `env_auth` choose `1` to enter your credentials

Now enter your `accesss_key_id` (your username)

Fill in the `secret_access_key` (your password)

If you set a region in MinIO, it goes in here.
The endpoint is the server URL from MinIO. In your LAN it would be `http://IP:PORT` (for example `http://192.168.0.70:9000`, it is the S3 API port, not the console !)
If you use a (sub)domain use `sub.domain.com`. No http or https needed in this case.

Simply skip `location_constraint` with enter

For `acl` you should use `1`

![rclone2](https://raw.githubusercontent.com/s3drive/app/master/resources/images/rclone/rclone2.png)


Serverside encryption is `none` (1)

Skip `sse_kms_key_id` with enter

`Advanced configuration` is not needed, just press enter

Now we get an overview of our configuration. If everything is okay press `y`

We are back at the start with our new remote. Let's try it out, before we encrypt.
Quit Rclone and test copy some files to your MinIO. Backup the configuration folder of my home directory.

`rclone copy .config minio:mybackup` 

![rclone3](https://raw.githubusercontent.com/s3drive/app/master/resources/images/rclone/rclone3.png)

`minio` is the remote name and `mybackup` is the bucket name in MinIO.

`rclone copy .config minio:mybackup/myfolder`

#### Verify MinIO contents

![MinIO1.png](https://raw.githubusercontent.com/s3drive/app/master/resources/images/rclone/MinIO1.png)

It worked. If you don't want encryption you are ready to go with S3Drive. Login and you will see the files in your bucket.

It is time for encryption with Rclone for secure backups and setup S3Drive for easy managing the encrypted bucket.

### Setup encryption


| WARNING: It is _strongly_ advised to backup your encryption password as if you lose it we won't be able to help you recovering your files ! |
|---------------------------------------------------------------------------------------------------------------------------------------------|


Start Rclone configuration again with:

`rclone config`

We want to add a new encrypted remote, so press `n`, give it a name (secure in my case) and choose `Encrypt/Decrypt a remote` (number 10)

![rclonepart2-1.png](https://raw.githubusercontent.com/s3drive/app/master/resources/images/rclone/rclonepart2-1.png)

Next we need to tell Rclone which remote shall be encrypted. We only have one with the name minio and our bucket mybackup, so type `minio:mybackup`
Please always use a specific bucket and not the whole remote. 

You can use different subfolders, if you want to store encrypted backups from different sources.
It is recommended not to store encrypted and unencrypted data in the same folder ! 

If you have only one bucket and want to store encrypted backups and unencrypted data you can use a subfolder here too (minio:mybackup/myfolder)

For `filename_encryption` we use `1`, the same goes for the `directory_name_encryption`

Next please provide a secure password and store it in a safe place, as mentioned in the beginning ! <br>
Press `y` to use your own password or `g` to generate a new one. The password will **only be visible** this time.

S3Drive only supports one password for now, so just press "n" when asked for the second password. Next step is the advanced configuration, so press `y`

![rclonepart2-2.png](https://raw.githubusercontent.com/s3drive/app/master/resources/images/rclone/rclonepart2-2.png)

Skip `server_side_across_configs` with enter then press `2` for the encryption of the data.
Skip `pass_bad_blocks` again with enter and now choose `2` for encoding with base64. 

This is _important_, if you made a misstake you can always edit again, so don't worry

Last step is to skip `suffix` with enter and then press `n` because we don't need any further configuration.
Lastly you get an overview again, press `y` to accept this config and exit Rclone.


![rclonepart2-3.png](https://raw.githubusercontent.com/s3drive/app/master/resources/images/rclone/rclonepart2-3.png)

We are done on this side, clear your bucket, because we want to test our encryption

We will now use the name `secure` as our remote. Secure is the encryption and points to our MinIO remote in the bucket `mybackup`

`rclone copy .config secure:myfolder`

#### Verify at MinIO

Take a look in your MinIO browser. The same files are now stored in your bucket in a cryptical folder. The files are encrypted too.

![MinIO2.png](https://raw.githubusercontent.com/s3drive/app/master/resources/images/rclone/MinIO2.png)

## S3Drive setup
Finally we need to setup S3Drive, since we want to get access to our data.
Start the app and login with your credentials.

![s3-1.jpg](https://raw.githubusercontent.com/s3drive/app/master/resources/images/rclone/s3-1.jpg)

You see the exact encrypted folder like in the MinIO Console. 

![s3-2.jpg](https://raw.githubusercontent.com/s3drive/app/master/resources/images/rclone/s3-2.jpg)

Go to the settings and tap on E2E encryption. 

![s3-3.jpg](https://raw.githubusercontent.com/s3drive/app/master/resources/images/rclone/s3-3.jpg)

Press the start button and then type in the same password you have chosen for the encryption in Rclone. 

![s3-4.jpg](https://raw.githubusercontent.com/s3drive/app/master/resources/images/rclone/s3-4.jpg)

Go back to your bucket and you will see it is already readable. 

![s3-5.jpg](https://raw.githubusercontent.com/s3drive/app/master/resources/images/rclone/s3-5.jpg)  
![s3-6.jpg](https://raw.githubusercontent.com/s3drive/app/master/resources/images/rclone/s3-6.jpg)

All files you upload through the app are encrypted too.

### Custom Rclone configuration path
If you want to use Rclone in scripts with automatic encryption, you can specify the configuration file. For example you can use this:

```
#!/bin/bash
rclone copy .config secure:myfolder --config=/path/to/rclone.conf
```