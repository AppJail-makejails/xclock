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
xhost +
appjail run -s xclock_open xclock
```
#
### Arguments

* `xclock_tag` (default: `13.5`): see [#tags](#tags).
* `xclock_ajspec` (default: `gh+AppJail-makejails/xclock`): Entry point where the `appjail-ajspec(5)` file is located.

## Tags

| Tag    | Arch    | Version        | Type   |
| ------ | ------- | -------------- | ------ |
| `13.5` | `amd64` | `13.5-RELEASE` | `thin` |
| `14.3` | `amd64` | `14.3-RELEASE` | `thin` |
