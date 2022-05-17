+++
title='(2) NodeMCU FirmwareBurn lua固件支持'
tags=["IOT"]
categories=["IOT"]
date="2022-05-09T16:11:35+08:00"
toc=true
draft=false
hiddenFromHomePage= false
+++


### nodemcu lua固件烧录

1. 固件下载地址
https://nodemcu-build.com/

2. 烧录工具
https://github.com/marcelstoer/nodemcu-pyflasher

3. 选择需要的模块编译 
![](2022-05-09-16-30-45.png)

几分钟后邮箱会收到bin文件,进行烧录
![](2022-05-09-16-33-50.png)

4. ESPlorer 下载安装
https://github.com/4refr0nt/ESPlorer/releases

![](2022-05-10-11-27-23.png)

5. Test
```
led = 4
gpio.mode(led,gpio.OUTPUT,gpio.PULLUP)
gpio.write(led,gpio.LOW)
```

![](2022-05-10-11-29-26.png)

![](2022-05-10-11-39-18.png)


