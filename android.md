# Android

## [ADB](https://developer.android.com/studio/command-line/adb)

### [Port Forwarding](https://developer.android.com/studio/command-line/adb#forwardports)

You can use the `forward` command to set up arbitrary port forwarding, which forwards requests on a specific host port to a different port on a device. The following example sets up forwarding of host port 6100 to device port 7100:

```text
adb forward tcp:6100 tcp:7100
```

