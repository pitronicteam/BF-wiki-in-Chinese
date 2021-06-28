# 电调编程透传

### Betaflight BLHeli 电调透传

1. 将飞控连接到电脑并记下COM端口号
2. 打开BLHeliSuite，选择通信端口，并单击Connect
3. 在“Select ATMEL/SILABS Interface”菜单中选择“SILABS BLHeli Botloader\(Cleanflight\)”
4. 为电调通电
5. 单击“Read Settings”

现在可以更改电调设置或烧录新的固件了。

### Betaflight KISS 电调透传

Betaflight 3.1中的新功能允许您直接连接到KISS Flash Loader程序来烧写KISS电调。

注意：目前并不是所有飞控均支持该功能。

Flash Loader是一个Chrome应用程序，必须在启用了“开发人员模式”的情况下，在扩展程序中进行安装。

您并不能直接在Flash Loader应用程序中直接开启透传，而是需要在CLI中通过命令启用透传协议。

KISSESC\_flashloader可以在[第一篇Dshot线程](https://www.rcgroups.com/forums/showthread.php?2756129-Dshot-testing-a-new-digital-parallel-ESC-throttle-signal)的附件中找到。

1号电调的命令：escprog ki 1

2号电调的命令：escprog ki 2

同时对四个电调进行操作的命令：escprog ki 255

操作步骤非常简单：

* 在CLI中输入上述命令，然后点击“断开连接”按钮
* 转到KISS Flash Loader应用程序，并选择要连接的COM口
* 选择 USB-uart
* 选择Dshot hex文件进行烧写
* 您可以启用快速bootloader开关以加速烧写。有些人报告说快速模式无法工作，所以当遇到这种情况时，则建议关闭快速bootloader
* 给电调上电。在此步之前不要给电调上电
* 按下Flash按钮，然后你将会看到电调上的LED开始闪烁
* 拔下电池和USB线，现在你可以使用Dshot了

如果上述命令不起作用，则证明电调透传协议尚未编译进飞控固件。

### 当您在工作台插上电池进行测试或烧写电调固件、校准或打开配置程序时，应该始终使用限流保护器！







