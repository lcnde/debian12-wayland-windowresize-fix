# debian-12-wayland-window-resize-glitch-fix

This repo explains how to fix graphical issues that appear when resizing windows that belong to Gnome software (ex. file manager, calculator app, text editor, settings). The issue has been discussed [on this Github thread](https://github.com/NVIDIA/egl-wayland/issues/57) and has been resolved. 

Since Debian runs software that's a little bit older, this repo explains how to fix the issue for Debian 12.0.5 bookworm.

The problem is caused by the package `libnvidia-egl-wayland1:1.1.10`. We want to update it to version `1.1.13`. We can find the newer version  of the package in the Debian "trixie" repository, which is currently the "testing" branch. We are going to add this branch to our apt sources and install this single package from the trixie repos.

Create a backup before starting.

Open a terminal a type:

```
$ su -
```

The above command will give your terminal root permissions.

Now edit your sources list:

```
$ nano /etc/apt/sources.list
```

Add the following lines inside `/etc/apt/sources.list`:

```
deb http://deb.debian.org/debian/ testing main contrib
deb-src http://deb.debian.org/debian/ testing main contrib
```

Add configuration to specify your preference to update packages using the stable repo (if the file does not exist, create it):

```
$ nano /etc/apt/preferences.d/preferences
```

Add the following lines inside `/etc/apt/preferences.d/preferences`:

```
Package: *
Pin: release a=bookworm
Pin-Priority: 500

Package: *
Pin: release a=testing
Pin-Priority: 100
```

If you do `$ apt update` and the pin-priority is not configured well, you pull all the updates from the trixie repo. You can delete the fetched updates using this command:

```
$ rm -rf /var/lib/apt/lists/*
``` 

In conclusion, you can install `libnvidia-egl-wayland1` version `1.1.13`, which will fix the graphical glitch when resizing windows:

```
$ apt -t testing install libnvidia-egl-wayland1
```

After you're done, reboot your machine and the glitch should be gone.
