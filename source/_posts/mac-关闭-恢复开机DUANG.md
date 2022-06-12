---
title: mac 关闭/恢复开机DUANG
date: 2020-10-12 18:31:01
tags: Mac
---

#### macOS Big Sur关闭开机`DUANG`

```bash
sudo nvram StartupMute=%01
```

如果上面命令执行后还是有声音，继续执行

```bash
sudo nvram SystemAudioVolume="％80"
```

或

```bash
sudo nvram SystemAudioVolume="％00"
```

如果想恢复启动音默认设置，依次执行输入命令

```bash
sudo nvram -d StartupMute
```

```bash
sudo nvram -d SystemAudioVolume
```



