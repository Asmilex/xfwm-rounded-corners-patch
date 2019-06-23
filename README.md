## Xfwm Rounded Corners Patch
This is a patch for Xfwm4 that allows drawing windows with rounded corners.

It was adapted from a [patch targeting Openbox](https://github.com/dylanaraps/openbox-patched/blob/master/openbox-3.6.2-rounded-corners.patch) with some advanced copy-pasting techniques.

The patch in its current state can be applied to an Xfwm 4.12.5 codebase and was not tested on other versions.

### Screenshot
Few applications with default radius for rounded corners and disabled shadows for clarity.

![](https://i.imgur.com/L84NfF2.png)

### Applying the patch
First, get the Xfwm's source code and checkout the tag corresponding to the 4.12.5 release:
```
git clone https://github.com/xfce-mirror/xfwm4.git
cd xfwm4
git checkout xfwm4-4.12.5
```

Then, make sure you are able to build the original source code without errors (hunt and install dependencies on your own):
```
./autogen.sh --prefix=/usr
make
```

Finally, drop the patch file into the current directory, apply it and build the modified source code:
```
git apply xfwm-4.12.5-rounded-corners.patch
make
```

After that, there should be an `xfwm4` binary in the `src` subdirectory. You can run it as follows:
```
./src/xfwm4 --replace &
```

### Configuring the rounded corners
By default, the rounded corners have a radius of 12 px and are disabled for maximized windows. Also, the window decorations such as titlebar and borders are hidden, because it doesn't look any good* with current implementation.

You can change the radius and enable maximized rounding with these commands:

```
xfconf-query -c xfwm4 -p /general/rounded_corners_radius -s 18
xfconf-query -c xfwm4 -p /general/rounded_corners_maximized -s true
```

Setting the radius value to 0 disables the effect altogether and restores window decorations.

In order to apply the changes, you have to restart the Xfwm:
```
./src/xfwm4 --replace &
```

<sub>* Yeah, this patch is more about the aesthetics than functionality.</sub>
