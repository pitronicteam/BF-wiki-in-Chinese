# Betaflight 4.1

**我们已经迟到了，所以让我们长话短说！**

如您所料，我们在此版本中添加了许多新的功能、新的改进以及对许多新目标的支持。

有关新功能的详细列表请参照下文。

为确保您已安装最新版本的固件，请访问[此页面](https://github.com/betaflight/betaflight-configurator/releases)并确保您已安装最新版本的Betaflight配置器，**然后再更新固件**。

为了充分发挥飞行性能，得到全部的性能提升，请阅读此[调参技巧](../tiao-can-ji-qiao/betaflight-4.1.md)。

如果您从较早版本的Betaflight升级而来，请阅读以下部分，其中包含了您可能需要在配置器中更改的选项列表。

我们已尽力使此版本没有错误。如果您仍然发现了一些错误，请在Github代码库内打开一个[issue](https://github.com/betaflight/betaflight/issues)向我们报告。

**祝您玩的开心**  


### **升级时需要注意的重要信息**

* Betaflight 4.1对下载和安装固件的方式做出了根本性的变更。而最新版本的[Betaflight配置程序](https://github.com/betaflight/betaflight-configurator/releases)10.6.0中包含了用于支持此功能所需的更改。**因此，重要的事是您必须更新到10.6.0或更高版本（安装说明**[**在这里**](https://github.com/betaflight/betaflight-configurator#installation)**），以获得最新版本固件的安装程序。**
* [黑盒日志查看器](https://github.com/betaflight/blackbox-log-viewer/releases)将更新到3.4.0，以搭配Betaflight4.1固件一起使用（安装说明[在这里](https://github.com/betaflight/blackbox-log-viewer#installation)）。
* 适用于遥控器的[Betaflight Lua脚本](https://github.com/betaflight/betaflight-tx-lua-scripts/releases)版本将更新到1.4.0，其中包括了Betaflight 4.1附带的更改（安装说明[在这里](https://github.com/betaflight/betaflight-tx-lua-scripts#installing)）。
* 双向Dshot（基于RPM回传的滤波器组）已得到改进，现在可用于BLHeli\_32（32.7+）和BLHeli\_S硬件（刷写JESC固件）。请使用《双向Dshot和RPM滤波器》进行设置
* 如您所料，这里有Betaflight详细的[调参技巧](../tiao-can-ji-qiao/betaflight-4.1.md)。请搭配调参技巧，或使用Betaflight配置器10.6.0中新的调参滑块来调整参数。**请不要直接粘贴早期固件中的参数配置。**一些默认参数已经更改，并且某些参数将以不同的方式参与控制，因此旧的参数设置无法在Betaflight 4.1中正常使用。
* 已经引入可以全配置的图传控制组件\([图传表](http://mp.weixin.qq.com/s?__biz=Mzg4OTAzNzM5MA==&mid=2247484653&idx=1&sn=1d0ad0e942dc6c72aa7544623f1832f7&chksm=cff0b3ccf8873adae06185c0715ed8dea7412f132eb56f2ce7c319aebd21f739b93a685b3ff5&scene=21#wechat_redirect)\)。现在在刷新固件之后，若想通过Betaflight来控制图传，您必须加载适用于图传的符合您所在国家/地区法律法规的图传表配置。从10.6.0开始，配置器将支持从文件加载图传表。请检阅Github上的文档\(《图传表》\)来获取有关如何查找合适的图传表，和安装图传表的说明。
* 我们对OSD字体进行了一些优化，并对一些字符进行了改进。为了在Betaflight 4.1上获得能够正常工作的字体，您需要将OSD的字库升级到最新版本\(在10.6.0或更新版本的配置器中可用\)。
* 正如之前所宣布的那样，Betaflight 4.1删除了对F3飞控的支持。

### **主要功能**

* 改进了的全新的前馈2.0
* 重写了双向Dshot协议
* 使用RPM回传进行动态怠速管理
* 完全可配置的图传控制功能——图传表 

### **次要功能**

* 支持Spektrum SRXL2串行协议
* 支持向特定飞控导入自定义默认值
* 支持任意陀螺仪与磁力计对齐

