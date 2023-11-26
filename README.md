# xclock

Analog and digital clock for X.

wikipedia.org/wiki/X.Org\_Foundation#xutils

![xclock logo](https://i.ibb.co/ggXHgXg/xclock.png)

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
ARG network?
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
appjail makejail -j xclock
# or use a network explicitly
appjail makejail -j xclock -- --network development
```

After Makejail builds the jail, you can run xclock using the `xclock_open` custom stage after starting the jail:

```sh
appjail start xclock
appjail run -s xclock_open xclock
```

## How to build the Image

Make any changes you want to your image.

```
INCLUDE options/network.makejail
INCLUDE gh+AppJail-makejails/xclock --file build.makejail
```

Build the jail:

```sh
appjail makejail -j xclock
```

Remove unportable or unnecessary files and directories and export the jail:

```sh
appjail stop xclock
appjail cmd local xclock sh -c "rm -f var/log/*"
appjail cmd local xclock sh -c "rm -f var/cache/pkg/*"
appjail cmd local xclock sh -c "rm -f var/run/*"
appjail cmd local xclock vi etc/rc.conf
appjail image export xclock
```

## Tags

| Tag    | Arch    | Version        | Type   |
| ------ | ------- | -------------- | ------ |
| `13.2` | `amd64` | `13.2-RELEASE` | `thin` |
| `14.0` | `amd64` | `14.0-RELEASE` | `thin` |
