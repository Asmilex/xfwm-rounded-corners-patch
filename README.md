## Xfwm Rounded Corners Patch
This is a patch for Xfwm4 that allows drawing windows with rounded corners.

It was adapted from a [patch targeting Openbox](https://github.com/dylanaraps/openbox-patched/blob/master/openbox-3.6.2-rounded-corners.patch) with some advanced copy-pasting techniques 🤭.

The patch in its current state can be applied to an Xfwm 4.12.5 or 4.13.2 codebase (there are two adequate files).

### Screenshot
Sample windows with 12 px radius for rounded corners, hidden decorations and disabled shadows for clarity.

![](https://i.imgur.com/L84NfF2.png)

### Applying the patch
First, get the Xfwm's source code and checkout the tag corresponding to a supported version:
```
git clone https://github.com/xfce-mirror/xfwm4.git
cd xfwm4
git checkout xfwm4-4.12.5
```

Then, make sure you can build the original source code without errors (hunt and install dependencies on your own):
```
./autogen.sh --prefix=/usr
make
```

Finally, drop the adequate patch file into the current directory, apply it and build the modified source code:
```
git apply xfwm-4.12.5-rounded-corners.patch
make
```

After that, there should be an `xfwm4` binary in the `src` subdirectory. You can run it as follows:
```
./src/xfwm4 --replace &
```

The easiest way to run it on a daily basis, without messing up your existing setup, is to create an autostart entry that launches the new `xfwm4` binary while stopping the original one. For example, it's possible to do it like this:
```
tee ~/.config/autostart/xfwm-rounded-corners.desktop <<EOF
[Desktop Entry]
Type=Application
Name=xfwm-rounded-corners
Exec=$(pwd)/src/xfwm4 --replace
OnlyShowIn=XFCE;
EOF
```

### Configuring the rounded corners
By default, the rounded corners have a radius of 8 px and are disabled for maximized windows. Also, window decorations are visible by default, but mind that most Xfwm themes won't look good with rounded corners because they provide side and bottom borders. Themes with no borders, but only with titlebar, should look fine though.

You can change the radius, enable maximized rounding or hide window decorations with these commands:

```
xfconf-query -c xfwm4 -p /general/rounded_corners_radius -s 12
xfconf-query -c xfwm4 -p /general/rounded_corners_maximized -s true
xfconf-query -c xfwm4 -p /general/rounded_corners_keep_decorations -s false
```

Setting the radius value to 0 disables the effect and restores original window shapes.
