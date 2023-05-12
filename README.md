# xclock

Analog and digital clock for X.

wikipedia.org/wiki/X.Org_Foundation#xutils

![xclock logo](https://imgur.com/S9mphOL.png)

## How to use this Makejail

```
INCLUDE options/network.makejail
INCLUDE gh+AppJail-makejails/xclock

OPTION copydir=files
OPTION file=/etc/rc.conf
OPTION x11
```

Where `options/network.makejail` are the options that suit your environment, for example:

```
ARG network
ARG interface=xclock

OPTION virtualnet=${network}:${interface} default
OPTION nat
```

The tree structure of the `files/` directory is as follows:

```
# tree -pug files/
[drwxr-xr-x root     wheel   ]  files/
└── [drwxr-xr-x root     wheel   ]  etc
    └── [-rw-r--r-- root     wheel   ]  rc.conf

1 directory, 1 file
```

Where `rc.conf` is your custom `rc.conf(5)` file, for example:

```
clear_tmp_X="NO"
```

The `rc.conf(5)` file above sets `clear_tmp_X` to `NO` to not remove the sockets and various related files before the jail starts.

Open a shell and run `appjail makejail` and `appjail start`:

```sh
appjail makejail -j xclock -- --network development
appjail start xclock
```

After Makejail builds the jail, you can run xclock using the `xclock_open` custom stage:

```sh
appjail run -s xclock_open xclock
```
