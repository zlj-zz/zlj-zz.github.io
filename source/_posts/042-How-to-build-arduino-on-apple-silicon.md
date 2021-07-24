---
title: mac m1 搭建 arduino 环境
date: 2021-04-13
category:
---


前前后后弄了几次 arduino 环境，这次就以 **mac m1** 为例，来做一个笔记。

---

# 搭建 arduino

首先，第一步肯定是安装 arduino，这直接去官网下载安装就好了。地址放在下面：
[https://www.arduino.cc/en/software](https://www.arduino.cc/en/software)

安装完成后，如下图打开 arduino 的设置进行配置。

![](https://img-blog.csdnimg.cn/20210413104540306.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70)
配置，board manager 的 url，这里我使用国内源。

![](https://img-blog.csdnimg.cn/20210413105101949.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70)
```
https://dl.espressif.com/dl/package_esp32_index.json
https://m5stack.oss-cn-shenzhen.aliyuncs.com/resource/arduino/package_m5stack_index.json
```

到这里，如果没有什么意外就可以使用了。

---

在我尝试最小 demo 编译时，报出了 `ValueError: dlsym(RTLD_DEFAULT, kIOMasterPortDefault): symbol not found`

在为了解决这个问题的时候，我找到有人同样在 BigSur 中出现这样的问题，并给出了解决方法，我尝试后也成功了，但我还不清楚这样可行的原因。

他的方法就是替换 `esptool` 文件，arduino 默认使用的是 二进制的 `esptool` 而不是 `esptool.py`。

替换步骤：

- 点击 Arduino 菜单的 Preferences，打开设置
- 找到 `[ArduinoLibraryPath]/preferences.txt` ，然后点击打开 ArduinoLibrary 目录
![](https://img-blog.csdnimg.cn/20210413110645267.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70)

- 接着打开 `packages/esp32/hardware/esp32/1.x.x` 目录下的 `platform.txt`
- 找到 第7行，将 `tools.esptool_py.cmd=esptool` 替换为 `tools.esptool_py.cmd=esptool.py` 然后保存
![](https://img-blog.csdnimg.cn/20210413110856591.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70)
- 然后找到 `packages/esp32/hardware/esp32/1.x.x/tools` 目录
- 把里面的 `esptool.py` 复制到 `ArduinoLibraryFolder/packages/esp32/tools/esptool_py/2.x.x`中
- 最后重新打开 arduino，你应该可以成功编译了

---

# 配置 vscode
首先，你要在插件中找到 arduino 的插件，然后安装它。
![](https://img-blog.csdnimg.cn/20210413203905616.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70)安装好后，打开 Arduino 的文件，就可以看到右上角有两个相关按钮。
![](https://img-blog.csdnimg.cn/20210413204303305.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70)
接着，要在设置中配置插件。最主要的就是这个 `Command path`，也就是arduino的启动位置。
![](https://img-blog.csdnimg.cn/20210413204139410.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70)这个时候就已经配置好了。在烧录前要记得选择板子和串口等。底部栏有快捷入口。
![](https://img-blog.csdnimg.cn/20210413204649435.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70)同时，你项目下的 `.vscode` 目录中会生成几个配置文件，也可以直接修改它们。
![](https://img-blog.csdnimg.cn/20210413204830105.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70)![](https://img-blog.csdnimg.cn/20210413204849395.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70)
![](https://img-blog.csdnimg.cn/20210413204909424.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70)







