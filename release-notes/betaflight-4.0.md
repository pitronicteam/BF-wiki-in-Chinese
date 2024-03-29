# Betaflight 4.0

**它花费了我们很长时间，但它现在已经正式发布了，而且它有很多新东西！**

当我们去年8月份发布3.5时，Betaflight支持的目标数量达到了150个，维护它们已成为一项繁重的工作，占用了我们很多时间。很明显，我们必须做点什么。我们一直在努力改变Betaflight的架构，以便在未来可以让不同的飞控板运行着相同的固件，基于这个原因，我们决定在下一个版本发布之前完成这些更改，由于这一根本性改变，下一版本号将变成4.0。

当接近Betaflight 4.0原定发布日期时，我们意识到，我们还没有准备好。于是我们决定将发布日期拖延三个月，以便能够完成那些已经开始了的工作。

所以现在，这些被称之为“统一目标”的东西，被我们称为新的“一个固件适用于多种飞控”的技术，在Betaflight4.0中已经成为了现实。这其中仍然有一些工作要做，将统一固件的配置文件添加到Betaflight配置器中，但一旦完成，我们将能够允许制造商在配置器中为任意飞控板或者RTF产品直接提供固件。

正如您对Betaflight所期望的那样，我们已经对飞行性能进行了一些新的和令人兴奋的改进，例如基于电调RPM的滤波器，使用D_Min对D增益进行动态管理，以及基于油门的动态陀螺仪和Dterm低通滤波器。

而且，正如所期望的那样，我们还增加了一些与飞行无关的新功能，例如弹射起飞模式、OSD配置文件切换以及对板载SPI接收机的支持。

有关新功能的详细列表请参照下文。

为了充分发挥飞行性能，得到全部的性能提升，请阅读[这些调参技巧](../tuning-notes/betaflight-4.0.md)。

如果您从较早版本的Betaflight升级而来，请阅读以下部分，其中包含了您可能需要在配置器中更改的选项列表。

我们已尽力使此版本没有错误。如果您仍然发现了一些错误，请在Github代码库内打开一个issues向我们报告。

哈哈哈哈（保持健康强壮）

### **升级时需要注意的重要信息**

* 此版本中的许多更改和改进都需要更改Betaflight配置程序以匹配功能。这些更改已经添加到betaflight-configurator10.5.0中。请尽快将您的配置器更新到10.5.0版本（或更高）。\

* 黑盒日志查看器将更新到3.3.0，以搭配Betaflight4.0固件一起使用。请尽快更新黑盒日志查看器。
* Betaflight 4.0中有许多与飞行性能相关的改进。因此，直接将旧版Betaflight的参数备份到Betaflight 4.0中很可能会导致飞行性能不佳！对于大多数穿越机来说，默认配置的性能表现应该相当优秀，但那些想要调整其参数以获得最佳性能体验的用户，应仔细阅读《4.0调参指南》，以了解有关调整Betaflight 4.0所需要的一切。
* 已经对“零油门”的死区配置函数“min_check”进行了修复。 在之前，死区的实际值基本上是将设置值进行了翻倍。所以，为了使零油门的死区保持与原来相同的范围，**你需要将配置过的min_check值翻倍**（偏移值为1000。例如：1035=>1070,1070=>1140）。对于将死区范围设置的非常小的用户来说，若不重新配置min_check，则可能导致在修复后的新版本中，即使遥控器上的油门最低，实际输出的油门值也永远不可能到达“零油门”的死区范围，并且飞控将以“油门未到最小值”的原因禁止解锁。
* **OSD内有了一个新的“摇杆叠层显示”图标元素，用于在OSD内叠层显示当前摇杆位置。**为了能正常使用它，需要将加载到OSD上的字库更新到最新版本（此操作将可以在10.5.0或更高版配置器中实现）。
* **OSD图标“反乌龟箭头”的功能被扩展**：当飞行器没有处于反乌龟模式时也该图标能够被激活。但只有当设置了最小解锁角度，并且由于飞行器当前角度高于最小解锁角度而无法解锁时，反乌龟图标才会显示。**这用于向飞行员展示是由于哪个方向的角度过大而导致无法解锁**，并且允许他们通过激活反乌龟模式来改变当前的角度。
* 作为引入统一目标的一部分工作，现在“resource”有了两个用于资源管理的新子命令：timer和dma。就像resource可用于给引脚分配功能一样，timer可用于给引脚分配定时器，dma可用于将DMA资源分配给子系统和引脚（前提是他们已经被分配了定时器）。重要提示：由于DMA通过定时器连接到引脚，因此必须先为引脚分配timer，然后才能分配dma。
* 作为合并CLI资源管理命令的一部分，resource list命令已被重新命名为 resource show。这将使得它能够与新的dam show和timer show命令格式相统一。
* 将一些函数重命名以使其更匹配函数所代表的实际意义：p_level=>angle_level_strength，i_level=>horizon_level_strength，d_level=>horizon_transition
* 统一目标中的陀螺仪配置适用于所有陀螺仪模型。这意味着对于一些使用多个陀螺仪（这些陀螺仪具有不同的安装方向甚至是不同的型号）的飞控来说，从Betaflight 4.0开始需要手动设置gyro\_1\_sensor_align（双陀螺仪则还需要设置gyro\_2\_sensor_align）以匹配飞控上的陀螺仪方向。这是一个临时的解决方法，统一目标配置中将具有陀螺仪的正确方向。
*   遗憾的是，由于飞控核心功能中的bug修正，导致固件大小超出了F3芯片的可用空间。所以必须从F3飞控固件中删除一些功能，收到影响的飞控有：

    AIORACERF3,BETAFLIGHTF3,CHEBUZZF3,CRAZYBEEF3FR,FURYF3,FURYF3OSD,IMPULSERCF3,LUX_RACE,LUXV2\_RACE,MIDELICF3,OMNIBUS,RACEBASE,RMDO,SIRINFPV,SPRACINGF3,SPRACINGF3MINI,SPRACINGF3NEO,STM32F3DISCOVERY
* 除上述要点外，还需要从F3飞控固件中删除“智能前馈”、“Simonk电调配置支持协议”。
* 为了进一步增加可用空间，LED灯条显示状态的功能也从F3飞控中被移除。使用LED灯条配置文件来代替，可用于将LED灯条配置为固定颜色。F4/F7仍保留完整的LED功能，同时也提供LED配置文件切换功能，以便在OSD中切换不同的LED灯条配置。
* 由于上述三条措施并不足以防止固件在F3飞控上溢出，因此，将F3飞控分为多个级别，逐级进行“功能剔除”。这将使大多数F3飞控具有的功能数量变得更少。

### **主要功能更新**

* 高保真实时电调转速回传(ESC RPM)，以及基于电机转速的RPM陷波滤波器(RPM Notch Filter)
* 基于油门的陀螺仪动态低通滤波器(Lowpass Filter)和Dterm动态低通滤波器
* 弹射起飞模式
* 可切换使用的OSD配置文件
* 支持SPI协议的Spektrum接收机
* 添加了统一通用目标

### **次要功能更新**

* 级联动态陷波滤波器
* 推力曲线线性化校正
* 整合式偏航控
* 可切换使用的LED灯条配置文件
* OSD中的摇杆位置叠层显示
* 基于电池芯片数(4s,5s,6s…)的PID自动切换功能
* 支持在使用CC2500芯片的硬件上使用Futaba SFHSS协议
* 支持以STM32F765xx为主芯片的飞控
* 通过HoTT文字回传模式更改配置
