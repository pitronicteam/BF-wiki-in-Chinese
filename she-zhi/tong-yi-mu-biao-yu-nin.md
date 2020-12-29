# 统一目标与您

* 确定要使用的飞控目标。  ![](../.gitbook/assets/identify_target.png) 

CLI获取飞控目标名称的另一种方法。

```text
# version
# Betaflight / STM32F405 (S405) 4.1.0 Oct 16 2019 / 11:57:16 (c37a7c91a) MSP API: 1.42
# manufacturer_id: HBRO   board_name: KAKUTEF4V2   custom defaults: YES
```

是的，芯片说它自己是STM32F405，但这并不意味着整张飞控板也叫这个名字。检查目标名称（target）

* 选择你的目标

  ```text
  MATEKF411 (MTKS)  <-- 统一目标
  MATEKF411 (Legacy)
  ```

常见问题：这四个字母是什么意思？他们指的是飞控板的制造商，该列表可在[生产制造商列表](https://github.com/betaflight/unified-targets/blob/master/Manufacturers.md)中找到。

提示：记得在刷入新版本固件之前保存备份您的配置，比如使用`diff`

* 点击`从网络加载固件`然后点击`烧写固件`
* 连接到配置程序，在出现下图时点击`应用自定义默认配置`：  ![](../.gitbook/assets/apply_custom_defaults_prompt.png) 

如果您在使用统一目标时遇到问题，请尝试使用旧版飞控目标，如果统一目标缺少应有的任何内容，请在[这里](https://github.com/betaflight/betaflight/issues)提交报告。

待办事项：例如，那个目标？还需要一些图片。

### 使用统一目标的提示

#### 使用固件

将统一目标代码仓库副本保存到您的本地计算机上，下面将使用`MTKS-MATEKF411.config`作为样例。

在文本编辑器中打开文件，然后查看第一行。

> \# Betaflight / **STM32F411** \(S411\) 4.1.0 Jun 25 2019 / 10:27:57 \(2a6e94d03\) MSP API: 1.42

在这种情况下，使用的处理器是`STM32F411`，因此在编译目标时要使用`make TARGET=STM32F411`

#### 与配置程序搭配使用并烧录

首先在配置程序中加载`.config`文件，然后加载`betaflight_4.x.x_STM32F411.hex`，然后烧写固件。在第一次连接时，系统将提示您应用自定义默认配置，就像常规的烧写过程一样。

#### make\_config\_hex.sh

make\_config\_hex.sh是位于src/utils下的脚本，额可用于将统一目标配置与固件结合在一起。组合后的.hex可与使用同一飞控板的其他用户相互分享。使用组合.hex的用户会像正常烧录一样 ，出现应用自定义默认配置的提示。

`srec_cat`程序是[srecord](http://srecord.sourceforge.net/)的一部分，它可以在ubuntu中的Universe下使用。  
`apt install srecord`

似乎无法在Windows下使用这个程序，但确实有适用于windows的使用说明

请参阅[这里](https://github.com/betaflight/betaflight/blob/master/src/utils/make_config_hex.sh)以获取更多信息

