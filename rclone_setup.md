## Quickstart guide for using rclone to encrypted data in MinIO and use it with S3Drive

**It is _strongly_ recommended to store the used password and the rclone.conf in a safe place! If you do not have backups, no one can recover your encrypted files!**

MinIO should be running and a bucket should be created already!

First we need to install rclone. Ubuntu 22.04 and Debian 12 come with Rclone 1.53, but you need **1.63** for base64 file encryption. 
On Debian or Ubuntu you can use:

`wget https://github.com/rclone/rclone/releases/download/v1.63.1/rclone-v1.63.1-linux-amd64.deb && sudo apt install ./rclone-v1.63.1-linux-amd64.deb`

Fedora, CentOS and Redhat:

`sudo dnf install https://github.com/rclone/rclone/releases/download/v1.63.1/rclone-v1.63.1-linux-amd64.rpm`

Next we need to create our config file for rclone.

`rclone config`


rclone will create a config file in your home directory/.config/rclone.conf

Just type `n` for a new remote, then give it a name. In this case I call it `minio`

Next we need to choose which backend shall be used. MinIO is Amazon S3 compilant storage, so we type `4`
AWS, Backblaze and so on work similar. Choose the number of the backened you want to setup.

![rclone1](https://github.com/Morethanevil/s3drive/blob/master/resources/images/rclone/rclone1.png)

Again choose Minio with the number `7`

In `env_auth` choose `1` to enter your credentials

Now enter your `accesss_key_id` (your username)

Fill in the `secret_access_key` (your password)

If you set a region in Minio, it goes in here.
The endpoint is the server URL from Minio. In your LAN it would be `http://IP:PORT` (for example `http://192.168.0.70:9000`, it is the S3 API port, not the console!)
If you use a (sub)domain use `sub.domain.com`. No http or https needed in this case.

Simply skip `location_constraint` with enter

For `acl` you should use `1`

![rclone2](https://github.com/Morethanevil/s3drive/blob/master/resources/images/rclone/rclone2.png)


Serverside encryption is `none` (1)

Skip `sse_kms_key_id` with enter

`Advanced configuration` is not needed, just press enter

Now we get an overview of our configuration. If everything is okay press `y`

We are back at the start with our new remote. Let´s try it out, before we encrypt.
Quit rclone and let us copy some files to our MinIO. I want to backup the configurationfolder of my home directory.

`rclone copy .config minio:mybackup` 

![rclone3](https://github.com/Morethanevil/s3drive/blob/master/resources/images/rclone/rclone3.png)


Minio is the remotename and mybackup is the bucket in MinIO. You can use subfolders too, if you can only use one bucket

`rclone copy .config minio:mybackup/myfolder`

See your backupfiles in MinIO

![MinIO1.png](https://github.com/Morethanevil/s3drive/blob/master/resources/images/rclone/MinIO1.png)

It worked. If you don't want encryption you are ready to go with S3Drive. Login and you will see the files in your bucket.

It is time for encryption with rclone for secure backups and setup S3Drive for easy managing the encrypted bucket.

Start rclone configuration again with:

`rclone config`

We want to add a new encrypted remote, so press `n`, give it a name (secure in my case) and choose `Encrypt/Decrypt a remote` (number 10)

![rclonepart2-1.png](https://github.com/Morethanevil/s3drive/blob/master/resources/images/rclone/rclonepart2-1.png)

Next we need to tell rclone which remote shall be encrypted. We only have one with the name minio and our bucket mybackup, so type `minio:mybackup`
Please always use a specific bucket and not the whole remote. 

You can use different subfolders, if you want to store encrypted backups from different sources.
It is recommended not to store encrypted and uncrypted data in the same folder! 

If you have only one bucket and want to store encrypted backups and uncrypted data you can use a subfolder here too (minio:mybackup/myfolder)

For `filenam_encryption` we use `1`, the same goes for `directory_name_encryption`

Next please provide a secure password and store it in a safe place, as mentioned in the beginning! Press `y` to use your own password or `g` to genrate one. The password will **only be visible** this time.

S3Drive only supports one password for now, so just press "n" when asked for the second password. Next step is the advanced configuration, so press `y`

![rclonepart2-2.png](https://github.com/Morethanevil/s3drive/blob/master/resources/images/rclone/rclonepart2-2.png)

Skip `server_side_across_configs` with enter then press `2` for the encryption of the data.
Skip `pass_bad_blocks` again with enter and now choose `2` for encoding with base64. 

This is _important_, if you made a misstake you can always edit again, so don't worry

Last step is to skip `suffix` with enter and then press `n` because we don't need any further configuration.
Lastly you get an overview again, press `y` for accepting this config and exit rclone.


![rclonepart2-3.png](https://github.com/Morethanevil/s3drive/blob/master/resources/images/rclone/rclonepart2-3.png)

We are done on this side, clear your bucket, because we want to test our encryption

We will now use the name `secure` as our remote. Secure is the encryption and points to our MinIO remote in the bucket `mybackup`

`rclone copy .config secure:myfolder`

Take a look in your MinIO browser. The same files are now stored in your bucket in a cryptical folder. The files are encrypted too.

![MinIO2.png](https://github.com/Morethanevil/s3drive/blob/master/resources/images/rclone/MinIO2.png)

It worked! Finally we need to setup S3Drive, since we want to get access to our data.
Start the app and login with your credentials.

![s3-1.jpg](https://github.com/Morethanevil/s3drive/blob/master/resources/images/rclone/s3-1.jpg)

You see the exact encrypted folder like in the MinIO Console. 

![s3-2.jpg](https://github.com/Morethanevil/s3drive/blob/master/resources/images/rclone/s3-2.jpg)

Go to the settings and tap on E2E encryption. 

![s3-3.jpg](https://github.com/Morethanevil/s3drive/blob/master/resources/images/rclone/s3-3.jpg)

Press the start button and then type in the same password you have choosen for the encryption in rclone. 

![s3-4.jpg](https://github.com/Morethanevil/s3drive/blob/master/resources/images/rclone/s3-4.jpg)

Go back to your bucket and you will see it is already readable. 

![s3-5.jpg](https://github.com/Morethanevil/s3drive/blob/master/resources/images/rclone/s3-5.jpg)  
![s3-6.jpg](https://github.com/Morethanevil/s3drive/blob/master/resources/images/rclone/s3-6.jpg)

All files you upload through the app are encrypted too.

If you want to use rclone in scripts with automatic encryption, you can specify the configuration file. For example you can use this this:

```
#!/bin/bash
rclone copy .config secure:myfolder --config=/path/to/rclone.conf
```

