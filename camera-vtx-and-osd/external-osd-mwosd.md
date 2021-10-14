# 外部OSD / MWOSD

本指南适用于没有板载OSD和/或希望使用外挂OSD的用户。

### 标准的MWOSD安装和配置

#### 关于这种模式：

* MWOSD从FC请求原始数据，处理成人类可读的样式并显示出来
* 屏幕布局和显示羡慕有MWOSD GUI配置程序来配置

#### MWOSD配置

* MWOSD安装指南等可从MWOSD的wiki中找到
* 建议从MWOSD GUI 配置程序中安装最新固件
* 1.7及以后的版本中支持betaflight启用CMS菜单
* 如果您正在编译您自己的MWOSD固件，请确保`CANVAS_SUPPORT`已被编译

#### FC配置

* Betaflight 3.1及以后的绝大多数F3、F4和F7固件中都启用了CMS
* 应在betaflight 配置程序中禁用OSD
* 必须在与OSD相连接的UART上启用MSP
* 波特率必须与OSD相匹配。通常应为115000

如果您正在编译您自己的betaflight飞控固件：

* 必须启用`CMS`和`USE_MSP_DISPLAYPORT`或其等效选项
* 如果要控制某一功能，应在配置中启用

#### 激活菜单

* Betaflight的CMS菜单激活方法是`油门归中+偏航打左+俯仰最大`
* MWOSD的CMS菜单激活方法是`油门归中+偏航打右+俯仰最大`

### 非标OSD的DISPLAYPORT安装和配置

#### 关于这种模式：

* MWOSD的行为怪异，并将显示由飞控生成的屏幕
* OSD布局需要在Betaflight配置程序中的OSD选项卡中配置

限制：

* 屏幕的更新速率将比标准安装方式慢
* 如果需要此模式，请向MWOSD团队提出请求。需要少量的开发工作以解决屏幕闪烁的问题

在DISPLAYPORT方式下使用：

* 安装最新的MWOSD固件
* 仔细阅读FC fonts文档，并使用MWOSD GUI 配置程序将其安装到OSD上。使用MWOSD原生字库将会导致字符显示怪异
* 应在Betaflight 配置程序中启用OSD

如果您正在使用您自己编译的betaflight固件：

* 编译时使用附加选项。例如`OPTIONS=OSD USE_OSD_OVER_MSPDISPLAYPORT REVOLT`
* 这将启用OSD选项卡和完整的DISPLAYPORT支持

另请参阅： [CMS adjustment](https://github.com/betaflight/betaflight/wiki/OSD-and-CMS-Adjusting-Screen)
