# Android System
System (usually Linux UID 1000) is a special user that bypasses many permission checks in android. This user is highly priviledged and is what runs apps like Settings. This user can access many protected areas of your device. Below is stuff you can do if you get access to System (either by an exploit in Android (like [CVE 31317](https://github.com/oddbyte/CVE-2024-31317)), or by a exploit in a specific app that has runs as the System shared UID (like the [Samsung TTS exploit](https://www.google.com/search?q=samsung+tts+shell)).

If you have access to System and run `id` you should get back something like this:
```
$ id
uid=1000(system) gid=1000(system) groups=1000(system),1065(reserved_disk),3009(readproc) context=u:r:system_app:s0
```

## I got access to system, what can I do?

### Enable Multi-user on unsupported devices
You can enable Multi-User support on any android device (even ones like Samsung which do not show this feature to users by default) using the following commands:
```
$ setprop persist.sys.max_users 15000 # Sets the max users to 15000 (extremely overkill)
$ setprop persist.sys.show_multiuserui 1 # Tells Android to show the multiuserui (you can access this in the pull-down notification area, usually near the settings button)
```

## Below is technical information on what it can access:
## Properties:
`bluetooth.*`

`persist.bluetooth.*`

`debug.*`

`net.*`

`sys.*`

`persist.sys.*`

`log.tag.*`

`persist.log.tag`

You can find what selinux context goes to what properties [here](https://android.googlesource.com/platform/system/sepolicy/+/main/private/property_contexts) and what `system_app` can access [here](https://android.googlesource.com/platform/system/sepolicy/+/main/private/system_app.te)

`system_app` can access any system properties that are protected by the following selinux context:
```
Bluetooth properties

adaptive_haptics_prop
arm64_memtag_prop
bluetooth_a2dp_offload_prop
bluetooth_audio_hal_prop
bluetooth_prop
bluetooth_lea_mode_prop
exported_bluetooth_prop

System properties

debug_prop
system_prop
exported_system_prop
exported3_system_prop
gesture_prop
locale_prop
log_tag_prop
logd_prop
net_radio_prop
timezone_prop
drm_forcel3_prop
dynamic_system_prop

USB properties

usb_control_prop
usb_prop

Control properties

ctl_default_prop
ctl_bugreport_prop
``` 
