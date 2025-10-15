---
title: 在Ubuntu上搭建STM32开发环境（CUBEMX+Clion+Ozone）
date: 2025-10-05 20:55:00
tags:
    - stm32开发环境
    - ubuntu
    - 教程
excerpt: 在Ubuntu上使用CLion进行STM32开发的详细教程，涵盖环境配置、工具安装和项目设置等内容。
author: He Wenxuan
language: zh-CN
cover: https://youke1.picui.cn/s1/2025/10/05/68e2782b28c4b.png
license: CC BY-NC-SA 4.0
---
# 简述
本文主要介绍ubuntu环境下使用CUBEMX+Clion+Ozone进行stm32开发的环境配置，适用于初学者。至于为啥要用Clion而不是Keil或者IAR，主要是因为Clion是跨平台的IDE，且对CMake支持很好，且免费（学生教育认证），而Keil和IAR都是收费的(但是有不收费的手段doge)，并且只能在Windows下使用。

# 开发环境
- Ubuntu 24.04

# 下载并安装CUBEMX
1. 直接去[st官网](https://www.st.com/en/development-tools/stm32cubemx.html)下载最新版本的CUBEMX，应该需要你登录，没有账号的按指示注册一个登录即可，下载后解压到你想放的地方即可。
2. 进入解压后的文件夹，执行`./SetupSTM32CubeMX-x.xx.x `(可能名字会有区别，反正运行安装程序就完了)，会出现一个图形化界面，按提示安装即可。
3. 打开下载完成的Cubemx，登录第一步注册的账号后,点击`INSTALL/REMOVE`下载所需软件包即可，例如我使用RoboMaster的C板，则下载STM32F4系列的软件包即可。

# 下载并安装Clion
1. 直接去[官网](https://www.jetbrains.com/clion/download/?section=linux)下载最新版本的Clion安装包，下载后放到任意位置(推荐放到`/opt/clion`目录下)解压即可。
解压命令：
```bash
sudo tar -xvf CLion-*.tar.gz -C /opt/clion --strip=1
```
2. 进入解压后的文件夹，执行`./clion.sh`即可打开Clion，或者直接双击`clion`文件
3. 第一次打开会提示你输入激活码，直接选择`Evaluate for free`即可免费试用30天，或者直接进行学生教育认证，很简单的，去[Jetbrains官网](https://www.jetbrains.com/)按要求操作即可。
4. 进入Clion后，选择Settings → Build, Execution, Deployment → Toolchains，配置好C和C++编译器为`/usr/bin/arm-none-eabi-gcc`和`/usr/bin/arm-none-eabi-g++`（如果是其他路径选择对应路径即可）。这里配置是因为我clion自动查找的的gcc和g++并非arm-none-eabi版本的，导致后续编译会报错，但是我有同学是可以不用手动配置的，请更具实际情况选择。

# 安装gcc-arm-none-eabi编译器和cmake
1. 输入安装命令：
```bash
sudo apt update
sudo apt install gcc-arm-none-eabi
sudo apt install cmake -y
```
2. 验证安装是否成功
```bash
arm-none-eabi-gcc --version
cmake --version
```
有内容输出即可

# 安装openocd(可选)
1. 输入安装命令：
```bash
sudo apt install openocd -y
```
2. 验证安装是否成功
```bash
openocd --version
```
有内容输出即可

# 安装JLink和Ozone
1. 直接去[Segger官网](https://www.segger.com/downloads/jlink/)下载最新版本的JLink和Ozone安装包
解压命令：
```bash
sudo apt install ./JLink_Linux_V788e_x86_64.deb
sudo apt install ./Ozone_Linux_V788e_x86_64.deb
```
2. 验证安装是否成功
```bash
JLinkExe
```
有内容输出即可
```bash
Ozone
```
会启动Ozone即可
3. 官方教学里是建议将 JLink 目录添加到环境变量中，但笔者操作时忘记这一步发现也是可以的，也可能是最新版Jlink在下载的时候会自动添加？命令如下：
```bash
echo 'export PATH=$PATH:/opt/SEGGER/JLink' >> ~/.bashrc
source ~/.bashrc
```

# 点个灯试试
1. 打开CUBEMX，选择`New Project`，选择你要使用的芯片型号，例如我使用RoboMaster的C板，则选择`STM#32F4407IGH6`，然后点击`Start Project`。
2. 配置相应工程，这里只是环境配置教学，就不赘述了。配置好后点击`Project Manager`，然后选择`Toolchain/IDE`为`CubeIDE`，然后点击`Generate Code`即可生成代码。这里我有个疑问，我亲测如果选择`CMake`，生成的代码直接编译会过不去，`makefile`我没试过，所以建议直接选择`CubeIDE`。
3. 用Clion打开工程文件夹，首次先右键选择cmake点击`Build`，如果顺利的话会生成`.elf`文件在`Debug`文件夹下。
4. 连接好JLink和开发板，打开Ozone，`Device`选择`STM32F407IG`，`Peripheral`选择`/opt/SEGGER/Ozone_Vxxx/Config/Peripheral/STM32F407IG.svg`，然后点击`Next`。在接下来的窗口中`Target Interface`选择`SWD`，并点击下面识别到的Jlink编号，点击`Next`，最后选择对应的elf文件，后面一路点`Next`即可。
5. 点击`Start Debugging`，然后点击左上角的`Run`按钮，点亮LED灯即可。由于Clion目前对Jlink的烧录配置过程不是很友好，所以建议直接通过在ozoneDebug的方式进行烧录和调试。
