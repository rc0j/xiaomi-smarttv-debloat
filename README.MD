# Debloat Xiaomi Smart TV

I recently picked up a Xiaomi Smart TV from a local store as I needed a cheap TV for my room: https://www.mi.com/global/product/xiaomi-tv-a-43-fhd/. It's nothing fancy but gets the job done.

The TV is pretty bloated and slow at times, so I decided to completely debloat it, remove the default Google TV launcher, replace it with Projectivy Launcher, and do some other optimizations.

## Getting Started

First, we need to enable ADB access. On the TV, navigate to:

`Settings > System > About > Android TV OS Build`

Click the build number about 7 times to enable Developer Options.

Once Developer Options are enabled, navigate to it and enable USB Debugging:

`Settings > Developer Options > USB Debugging`

Next, install ADB on your PC/Laptop, open a terminal, and run:

```shell
adb connect <ip_of_the_tv>
```

You can find the TV's IP address in the settings. If the connection is successful, you'll see a prompt on your TV to authorize the connection. Allow it.

## Debloating Process

Once connected to the TV, we can start disabling packages and optimizing performance.

### Enter ADB Shell

Access the ADB shell where we'll run our commands:

```shell
adb shell
```

If successful, you should see:

```shell
$ adb shell
irobot:/ $
```

### Debloat List (Updated Dec 2025)

Copy and paste the following commands inside the shell.

**Uninstall**:

```shell
pm uninstall --user 0 -k com.miui.tv.analytics
pm uninstall --user 0 -k com.android.providers.calendar
pm uninstall --user 0 -k android.autoinstalls.config.xioami.mibox3
pm uninstall --user 0 -k com.mitv.gallery
pm uninstall --user 0 -k com.android.syncadapters.calendar
pm uninstall --user 0 -k com.google.android.play.games
pm uninstall --user 0 -k com.google.android.videos
pm uninstall --user 0 -k com.google.android.feedback
pm uninstall --user 0 -k com.xiaomi.mitv.mediaexplorer
pm uninstall --user 0 -k com.mitv.tvhome.michannel
pm uninstall --user 0 -k com.mitv.videoplayer
pm uninstall --user 0 -k com.xiaomi.mimusic2
pm uninstall --user 0 -k com.mitv.tvhome.atv
pm uninstall --user 0 -k com.amazon.amazonvideo.livingroom
pm uninstall --user 0 -k com.android.printspooler
pm uninstall --user 0 -k com.xiaomi.android.tvsetup.partnercustomizer
pm uninstall --user 0 -k com.google.android.tts
pm uninstall --user 0 -k com.mitv.tvhome.mitvplus
pm uninstall --user 0 -k com.google.android.youtube.tv
pm uninstall --user 0 -k com.google.android.youtube.tvmusic
pm uninstall --user 0 -k com.android.providers.contacts.auto_generated_rro_product__
pm uninstall --user 0 -k com.android.camera2
pm uninstall --user 0 -k com.xiaomo.tv.milegal
```

**Disable**:

```shell
pm disable-user com.google.android.tv.bugreportsender
pm disable-user com.google.android.videos
pm disable-user com.google.android.feedback
pm disable-user com.xiaomi.mitv.smartshare
pm disable-user com.android.printspooler
pm disable-user com.google.android.tts
pm disable-user com.google.android.youtube.tvmusic
```

### Clean Up Storage

Clear the data for every disabled package:

```shell
for p in `pm list packages -d -u` ; do pm clear ${p/package:/} ; done
```

Clear app cached data:

```shell
pm trim-caches 4096G
```

> Note: Run these commands inside the ADB shell.

### Additional Optimizations

Inside Developer Options on your TV:

1. Change all animations to 0.5x
2. Limit background apps to 0 or 1 (depending on your preference)

### Install Projectivy Launcher

Since I didn't log in with my Google account on the TV, I used `adb install` to install my apps.

To install the launcher, download the APK from the [GitHub releases](https://github.com/spocky/miproja1/releases) and run the following command.

Navigate to where the APK is downloaded and run:

```shell
adb install -r ProjectivyLauncher-4.68-c82-xda-release.apk
```

> Note: Replace the filename with the version you downloaded.

And that's it! Your Xiaomi Smart TV should now be significantly faster and less bloated.


----

### Custom apps

Youtube: https://github.com/reisxd/TizenTubeCobalt
Jellyfin client: https://github.com/damontecres/Wholphin

