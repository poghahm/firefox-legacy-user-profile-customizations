# Firefox Legacy User Profile Customizations

These scripts enable easy configuration of Firefox profile directories.

The main [`update`](bin/update) script writes these files:
*   `$PROFILE_DIR/user.js`
*   `$PROFILE_DIR/chrome/userChrome.css`
*   `$PROFILE_DIR/chrome/userContent.css`

To make changes to [`user.js`](user.js) and [`userChrome.css`](userChrome.css),
just edit the files.

The [`compile`](bin/compile) script compiles `userContent.css`
from the contents of the [`src`](src) directory.

The [`format`](bin/format) script formats the source files.

Everyone's preferences are different, so the best way to use this repository
is as a starter template that you customize to your needs.

## Introduction

The following terminal transcript shows
*   how to install prerequisites (`npm install`)
*   how to mark a profile directory as managed (`./run bin/init`)
*   how to update the configuration of managed profiles (`./run bin/update`)
*   what the compiled `userContent.css` file looks like (`cat userContent.css`)

*Restart Firefox to pick up the new configuration.*

```
pog@bootes:~/src
$ npm install
added 410 packages from 259 contributors and audited 1612 packages in 3.249s
found 0 vulnerabilities

pog@bootes:~/src
$ ./run bin/init
Usage:
  ./run bin/init PROFILE_DIR

Examples:
  ./run bin/init /home/pog/.mozilla/firefox/6ey5rbxk.Email
  ./run bin/init /home/pog/.mozilla/firefox/ea8cttuh.Restaurants
  ./run bin/init /home/pog/.mozilla/firefox/si4c2ymx.Shop
  ./run bin/init /home/pog/.mozilla/firefox/ttcviopc.default

1 pog@bootes:~/src
$ ./run bin/init /home/pog/.mozilla/firefox/6ey5rbxk.Email
Touching /home/pog/.mozilla/firefox/6ey5rbxk.Email/.Aing1poghahm2ji3NoevaiLuocie3hij

pog@bootes:~/src
$ ./run bin/init /home/pog/.mozilla/firefox/ttcviopc.default
Touching /home/pog/.mozilla/firefox/ttcviopc.default/.Aing1poghahm2ji3NoevaiLuocie3hij

pog@bootes:~/src
$ ./run bin/update
Writing /home/pog/.mozilla/firefox/6ey5rbxk.Email/user.js
Writing /home/pog/.mozilla/firefox/6ey5rbxk.Email/chrome/userChrome.css
Writing /home/pog/.mozilla/firefox/6ey5rbxk.Email/chrome/userContent.css
Writing /home/pog/.mozilla/firefox/ttcviopc.default/user.js
Writing /home/pog/.mozilla/firefox/ttcviopc.default/chrome/userChrome.css
Writing /home/pog/.mozilla/firefox/ttcviopc.default/chrome/userContent.css

pog@bootes:~/src
$ cat userContent.css
/* MAGIC:Aing1poghahm2ji3NoevaiLuocie3hij */

@-moz-document domain("aaa.example.com"){.header{animation-play-state:paused;-webkit-animation-play-state:paused}}
@-moz-document domain("bbb.example.com"){body,html{height:initial!important;overflow-y:initial!important}#wrapper,body{position:initial!important}}
@-moz-document domain("ccc.example.com"){#openFeedbackContainer{display:none!important}}
@-moz-document domain("ddd.example.com"){main+aside{display:none!important}}
@-moz-document domain("eee.example.com"){#toasts_mount_point,.c-toast{display:none!important}}
@-moz-document domain("fff.example.com"){.commentPopover{display:none!important}}
@-moz-document domain("ggg.example.com"){.a-expander-collapsed-height{max-height:initial!important;height:initial!important}.a-expander-partial-collapse-header{display:none!important}}
@-moz-document domain("hhh.example.com"){#spinnerContainer,.annotation,.ytp-ce-element,.ytp-chrome-top-buttons,ytd-popup-container{display:none!important}}
@-moz-document regexp("https?://subdomain\\..*"){#nps-survey-inline-dialog{display:none!important}}
@-moz-document url("about:privatebrowsing"){body{display:none!important}}
@-moz-document url("https://iii.example.com/doc/doc.html"){body{background-attachment:initial!important}}
@-moz-document url-prefix("about:reader"){.toolbar{display:none!important}}
@-moz-document url-prefix("https://jjj.example.com/manual/"){.collapse{display:initial!important}}
```

## Linux

It is easy to create a site-specific browser for Gnome for a profile
to run as a separate application,
including with a custom icon in the Alt+Tab switcher.

First create a desktop file:
```
$ cat ~/.local/share/applications/firefox-example.desktop
[Desktop Entry]
Name=Example
Comment=Example
Exec=firefox -P Example --no-remote --name "firefox-example"
Icon=example
Terminal=false
Type=Application
```

The option `--name "firefox-example"`
causes Gnome to look for the `Icon` entry
in the `firefox-example.desktop` file.

For Wayland, the option is `--name "firefox-example"`.

For X, the option is `--class "firefox-example"`.

Then put the custom icons in the right location:
```
$ fd example ~/.local/share/icons
/home/pog/.local/share/icons/hicolor/128x128/apps/example.png
/home/pog/.local/share/icons/hicolor/16x16/apps/example.png
/home/pog/.local/share/icons/hicolor/256x256/apps/example.png
/home/pog/.local/share/icons/hicolor/32x32/apps/example.png
/home/pog/.local/share/icons/hicolor/512x512/apps/example.png
```

Log out and log back in to pick up the new icons, or run
```
touch ~/.local/share/icons/ && sudo gtk-update-icon-cache
```

Here is an example of how to make the icons using ImageMagick:
```
for i in 128x128 16x16 256x256 32x32 512x512; do convert -resize $i original.png example-$i.png; done
for i in 128x128 16x16 256x256 32x32 512x512; do cp example-$i.png ~/.local/share/icons/hicolor/$i/apps/example.png; done
```

The Apple App Store on the web is a good source of icons.

## macOS

Custom Cmd+Tab switcher icons appear not to be possible on the Mac.

This post describes how to create a site-specific browser
for a Firefox profile for the Mac:

*   https://spf13.com/post/managing-multiple-firefox-profiles-in-os-x/

It is possible to set a custom icon for use in the dock,
but once launched it shows up in the Cmd+Tab switcher
with the normal Firefox icon.
