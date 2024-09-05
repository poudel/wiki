+++
title = "SSH"
weight = 21
+++

Specify a key when connecting 

```shell
ssh -i ~/.ssh/privatekey user@remoteaddr.tld
```

## RouterOS specific notes

MikroTik routers have [SSH enabled](https://help.mikrotik.com/docs/display/ROS/SSH) 
by default. 
To enable passwordless login, I had to upload my public key to the "File"
section and import it using the following command:

```shell
/user ssh-keys import public-key-file=id_rsa.pub user=admin
```

# Troubles

## WARNING: UNPROTECTED PRIVATE KEY FILE!

Happened to me because I'd downloaded the key files. The error prevents the file
from being used. The solution is to chmod it to 600 like this.[^1]

```shell
chmod 600 ~/.ssh/privatekey
```

This will allow read+write only from the user.


# Refs

[^1] https://tecadmin.net/ssh-with-private-key-warning-unprotected-private-key-file/