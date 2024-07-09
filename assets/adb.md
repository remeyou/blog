# ADB

## Frequent commands

```bash
adb devices

adb -s emulator-5554 shell
settings put global http_proxy 192.168.31.134:2080
settings put global http_proxy :0
# settings delete global http_proxy

adb start-server
adb kill-server
```
