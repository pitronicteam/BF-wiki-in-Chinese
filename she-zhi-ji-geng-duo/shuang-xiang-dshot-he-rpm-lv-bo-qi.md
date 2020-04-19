# 双向Dshot和RPM滤波器

### 近期公告

* 适用于F3飞控的BF 4.1定制版固件，[在这里获取！](https://github.com/joelucid/betaflight/releases)
* 适用于BLHeli\_S电调的带有双向Dshot的免费固件，[在这里获取！](https://github.com/JazzMaverick/BLHeli/tree/JazzMaverick-patch-1)
* **本站强烈不推荐使用JESC**，但您仍可以在[这里](https://jflight.net/)了解JESC。
* 现在，BLHeli\_32的32.7.0已完全支持双向Dshot。您可以直接从BLHeliSuite32中直接刷写，而无需手动下载固件文件。

### 介绍

使用基于RPM回传的滤波器（组）可以更有效地控制电机噪声。电机会更凉，对于折弯的桨的容忍度会更高，全油门下的声音听起来会更干净，且加速更快。

如果开启动态陷波滤波器，则可以将其槽口调窄，从而减少延迟。它也可以被从追踪电机噪声这一任务中解放出来，来处理机架共振。

在干净的飞机上，通常可以通过提高滤波器截止频率或直接禁用某些陀螺仪滤波器来改善低通滤波器带来的延迟。请在阅读调整指南之后，再仔细进行此项操作。

它使用了两种基本技术：

[双向Dshot](https://github.com/betaflight/betaflight/pull/7264)是Betaflight 4.x的一项新功能。它使飞控能够在每个电机的电调信号线上接受到准确的RPM回传。这项新功能并不需要额外的电调遥测信号线，也不需要额外的反向遥测通道。每个从飞控发出的Dshot命令帧包含一条命令，这条命令会向电调请求实时的eRPM传感器读数。飞控需要预先知道电机的磁极数，这样就可以将eRPM转换为RPM。

[RPM滤波器](https://github.com/betaflight/betaflight/pull/7271)，运行在陀螺仪信号和Dterm信号上，共计36个。利用RPM回传数据来实现对电机噪声以手术级精度过滤。默认情况下，飞控在Pitch、Roll和Yaw上各运行12个陷波滤波器\(Notch\)，用于过滤陀螺仪内的每个电机噪声的前三级谐波。

为了使RPM滤波器正常工作，电调必须支持双向Dshot协议，并且必须在CLI中启用双向Dshot功能。

刷有Betaflight 4.1的所有飞控与刷有最新版本固件的BLHeli\_32和BLHeli\_S电调均支持这两个功能。Betaflight 4.0已不再支持此功能。

### **解锁前的预检测**

如果已启用RPM滤波器，但一个或多个电调未提供有效的转速数据的话，则将在OSD上显示`RPMFILTER`标志，并阻止解锁（更多信息请参阅解锁序列与安全）。这是为了防止由于配置不完整或滤波器无法正常工作，而导致飞行不可控或者导致电机过热。请参阅下文，以确保对电调进行了正确配置，来支持此功能的开启。

### 电调配置

#### 双向Dshot固件

Betaflight 4.1中的双向Dshot协议与Betaflight 4.0中的不同（4.1中的可靠性更高）。电调固件版本需与您当前的飞控固件版本相匹配。

**对于BLHeli\_32电调**，双向Dshot是32.7.0中完全支持的功能。只需使用BLHeliSuite32刷写新版本固件即可。

**对于BLHeli\_S电调**，请点击[这里](https://github.com/JazzMaverick/BLHeli/tree/JazzMaverick-patch-1/BLHeli_S%20SiLabs)下载JazzMaverick的固件。使用开源的[blheli配置程序](https://github.com/blheli-configurator/blheli-configurator/releases)进行刷写。

### Betaflight配置

#### 电机磁极数

运行8k/8k时，请选择Dshot600。电调向飞控发送eRPM，需要利用电机的磁极数量以将其换算成RPM。这些信息可以在电机转子上找得到，而不是绕组所在的定子磁铁数量。标准的5寸\(穿越机\)电机有14个磁极，因此14是默认设置。较小的电机通常有12个磁极。如果电机磁极数不是14，请自行计数或者查看电机的规格，并且在Betaflight配置程序的配置页面内更改磁铁数量。

#### Dshot150，Dshot300还是Dshot600？

若PID速率为4k，例如8k/4k或4k/4k，请使用Dshot300以获得最高可靠性；当然Dshot600也可以。

若PID速率为8k，则必须使用Dshot600；若使用Dshot300，则将每两个PID周期更新一次电机转速。

在L型电调上（BLHeli\_S的L型号电调，其主控为EFM8BB1），强烈建议使用Dshot150，并使用2k/2k的控制速率。

#### 配置片段

当您使用Betaflight 4.1，则不再需要使用特殊配置代码。相反，只需要在Betaflight配置程序内“配置”页面中启用双向Dshot。

#### 验证配置

现在你的飞控已经开启了双向Dshot。您现在需要验证它是否真正在工作。为此，您需要重启飞控和电调。首先，将电调供电（插上电池），然后插入USB线缆。现在，在Betaflight配置程序中打开“电机”页面，您不应该看到`E: xxxx`显示为红色。旋转电机是，应该看到R: xxxx显示电机的实时转速。回传的错误百分比不应该超过1%。除非电机正在旋转，否则所有电机的RPM都应是0。

**重要说明**：如果仅通过USB线缆连接飞控，而未插入电池，则将在“电机”页面内看到E: 100%的故障提示。插入电池并等待电调初始化完成，此读数将下降至0%。拔出电池后，将继续显示0%。

### 调参

RPM滤波器可以在不增加太多延迟的情况下清除几乎所有的电机噪声，从而完成繁重的滤波工作。

但是，RPM滤波器无法消除因轴承、风和湍流带来的一般“垃圾”噪声。仍然需要使用低通滤波器来抑制这些噪声。

RPM滤波器也无法消除频率特性固定的机架共振噪声。最好开启动态陷波滤波器来抑制这些噪声。

由于现在不再需要使用动态陷波滤波器来控制电机噪声，因此需要重新配置动态陷波滤波器。在配置程序的“滤波”页面内，将动态陷波滤波范围设置为“中”，将“动态陷波滤波器宽度百分比”设置为0，将“动态陷波滤波器Q”设置为250。  
![](../.gitbook/assets/rpmfilter-dynamic_filter_4.1_settings.png) 

若您的飞机在现有情况下飞行良好，请不要在开启RPM滤波后的首次试飞时更改任何滤波器设置。对于已经有一定问题的机器和新机器来说，我们建议从4.1默认的低通滤波器开始调整。下面代码段将使用4.1默认滤波设置，在0油门时延迟约为3.5ms，在全油门时延迟约为1.1ms。

```text
# 4.1默认低通滤波器设置

set gyro_lowpass_type = PT1
set gyro_lowpass_hz = 200
set dyn_lpf_gyro_min_hz = 200
set dyn_lpf_gyro_max_hz = 500
set gyro_lowpass2_type = PT1
set gyro_lowpass2_hz = 250

set dterm_lowpass_type = PT1
set dterm_lowpass_hz = 100
set dyn_lpf_dterm_min_hz = 70
set dyn_lpf_dterm_max_hz = 170
set dterm_lowpass2_type = PT1
set dterm_lowpass2_hz = 150
```

如果您的电机运转良好，且声音听起来不错，则飞行状况良好，那么可以通过减小或关闭其他一些滤波器来降低滤波延迟。您可以：

* 将现有的Biquad滤波器（如果有的话）改为PT1
* 将低通滤波器的截止频率提高
* 禁用整个滤波器组（将其截止频率设为0）

每一个上述操作都将使更多的噪声透过滤波器进入PID，然后再传入电动机。

只能一次更改一个滤波器设置。每次更改之后，请务必进行一次悬停测试和一次短暂的试飞。仔细听电机声音，然后迅速降落以确认电机没有过热。然后重复进行更稳定的飞行，并再次降落检查电机温度。

在更改设置之前和之后均记录黑盒日志，然后使用PIDtoolbox来查看频谱图是非常有效的。黑盒日志可以帮助您来确定关闭不需要的滤波器，并可以在更改设置之后来检查是否突然获得了额外的噪声。

最好在进行任何更改之前使用`diff all`来保存当前设置，以便恢复。

### 试飞成功后提高低通滤波器

同时以相同比例提高所有滤波器截止频率，是最安全、最可靠的方法。

下面的代码片段将所有低通滤波器上移约50%，从而将总延迟降低到0油门时延迟约为2.3ms，全油门时延迟约为0.9ms：

```text
# 4.1低通滤波器设置提升1.5倍

set gyro_lowpass_type = PT1
set gyro_lowpass_hz = 300
set dyn_lpf_gyro_min_hz = 300
set dyn_lpf_gyro_max_hz = 900
set gyro_lowpass2_type = PT1
set gyro_lowpass2_hz = 350

set dterm_lowpass_type = PT1
set dterm_lowpass_hz = 150
set dyn_lpf_dterm_min_hz = 100
set dyn_lpf_dterm_max_hz = 250
set dterm_lowpass2_type = PT1
set dterm_lowpass2_hz = 200
```

如果您原先使用的是4.1默认滤波器设置，那么这是适度但较为大幅的改动。您将注意到洗桨情况得到了明显改善。如果在第一次解锁时，飞机发出刺耳的声音并在解锁时或悬停过程中跳起来，通常是您的滤波器截止频率过高，或者是因为您的D过高。在这种情况下不建议继续飞行。

下一步将是使用默认值的2倍的设定值。只有最干净的飞机才能适应这种滤波设置。但是，滤波器延迟将变为正常值的一般，这可以大幅抑制洗桨震颤。0油门时延迟约为1.7ms，全油门时约为0.65ms：

```text
# 4.1低通滤波器设置提升2倍

set gyro_lowpass_hz = 400
set dyn_lpf_gyro_max_hz = 1000
set dyn_lpf_gyro_min_hz = 400
set gyro_lowpass2_type = PT1
set gyro_lowpass2_hz = 500

set dterm_lowpass_type = PT1
set dterm_lowpass_hz = 200
set dyn_lpf_dterm_max_hz = 340
set dyn_lpf_dterm_min_hz = 140
set dterm_lowpass2_type = PT1
set dterm_lowpass2_hz = 300
```

### 完全禁用低通滤波器

要完全禁用低通滤波器，而又不烧毁电机，那么您的飞机的机械结构必须处于较为健康合理的状态：机臂未发生分层，螺丝未松动，陀螺仪芯片工作正常，电机轴承运转正常无噪声。

禁用滤波器将会产生相当大的影响，只能由熟悉滤波器的人员来完成。如上所述，将全部滤波器视作一个组，对其进行同步调整是较为安全的。

下列代码将完全禁用陀螺仪低通滤波1：

```text
set gyro_lowpass_hz = 0
set dyn_lpf_gyro_max_hz = 0
```

下列代码将完全禁用陀螺仪低通滤波2：

```text
set gyro_lowpass2_hz = 0
```

不建议完全禁用Dterm滤波器，因为这样做会很容易烧毁您的电机。将Dterm滤波器视作一个组来同步调整是较为安全的做法。

### 配置动态陷波滤波器

使用RPM滤波器配置片段将会将动态陷波滤波器重新配置为单个、槽口较窄的陷波滤波器，从而降低滤波延迟。

默认情况下，动态陷波范围设置为`AUTO`模式，并使用`陀螺仪动态低通滤波器最高截止频率`的值来选择动态陷波滤波器的运行范围。

由于机架共振的频率通常在动态陷波滤波器的`LOW`或`MEDIUM`范围内，所以最好在使用RPM滤波器时，手动配置动态陷波滤波器。如果您知道低于150hz的频段内没有机架共振噪声峰，则建议将范围设置为`MEDIUM`来降低延迟。

假设您知道低于某个频率的频段内没有共振噪声，而要使动态陷波滤波器中央频率始终大于某个最低频率，请相应地设置动态陷波滤波器最低频率。

例如，下列代码将使动态陷波滤波器运行范围调整为90-330Hz，并将槽口缩窄，陷波器槽口数量减小为1：

```text
set dyn_notch_range = LOW
set dyn_notch_width_percent = 0
set dyn_notch_q = 200
set dyn_notch_min_hz = 90
```

下列代码将使动态陷波滤波器运行范围调整为180-550Hz（单个窄槽口）：

```text
set dyn_notch_range = MEDIUM
set dyn_notch_width_percent = 0
set dyn_notch_q = 200
set dyn_notch_min_hz = 180
```

如果共振噪声在频谱上分布得较窄，则可以将动态陷波Q调整为250。共振带较宽或有多条共振带则可能需要将Q设为120-150。

最好将关闭动态滤波器前后的频谱图进行比较，以准确确定其性能。

可以在无共振的刚性结构机架上完全关闭动态陷波滤波器，但是应谨慎行事，最好在对开启前后进行对比观察，并最终确定其无意义的情况下进行飞行。

### 进阶主题

#### 基于计时器的双向Dshot

如果您的飞控支持，则可以使用我们的基于计时器的双向Dshot，来稍微降低开启双向Dshot后的CPU负载。在4.1之前，唯一可用的实现方法是基于DMA的双向Dshot。它的缺点是，它要求将某些飞控上的计时器和DMA通道进行重新映射后方可使用，并且不适用于所有飞控。

从4.1开始，您需要使用`set dshot_bitbang=off`来禁用默认的Bitbang方式的双向Dshot。如果要切换回bitbang方式的双向Dshot，请将其设置为`AUTO`。

#### 支持非bitbang的双向Dshot的飞控及配置代码片段



| 飞控目标 | 配置代码 | 注意 | 支持的电机 |  |
| :--- | :--- | :--- | :--- | :--- |
| AG3XF4 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | M1 - M4 \(tested Mister\_M\) |  |
| AIKONF4 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/AIKONF4-upgrade.cf) |  | M1 - M4 \(tested fujin\) |  |
| AIRBOTF7 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | M1-M4 \(tested by Asizon\) |  |
| ALIENFLIGHTNGF7 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/ALIENFLIGHTNGF7-upgrade.cf) | 无法使用M3，请使用M5-9来替代。LED与M1无法同时工作 | M1-M2, M4-M9 |  |
| ALIENWHOOP | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | M1-M4 |  |
| ANYFCF7 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | M1 M2 M3 M4 M5 M6 M9 |  |
| ANYFCM7 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/ANYFCM7-upgrade.cf) |  | M1 M2 M3 M4 M5 M7 M9 M10 |  |
| BETAFLIGHTF4 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | M1 - M4 ok \(tested Balint\) |  |
| CLRACINGF4 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/CLRACINGF4-upgrade.cf) |  | M1-M4 ok |  |
| CLRACINGF7 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/CLRACINGF7-upgrade.cf) | 4号电机不工作，请使用LED焊盘来代替。 | M1 M2 M3 M5 |  |
| CRAZYBEEF4DX | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | M1-M4 ok \(tested Noctaro\) |  |
| CRAZYBEEF4FR | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | M1-M4 ok \(tested joelucid\) |  |
| DALRCF4 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DALRCF4-upgrade.cf) |  | M1-M6 \(tested QuadMcFly\) |  |
| DALRCF722DUAL | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DALRCF722DUAL-upgrade.cf) |  | M1-M6. But either M5 or M6 |  |
| DYSF4PRO | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | M1-M4 \(tested BRadFPV\) |  |
| ELINF405 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | M1-M4 \(tested elin-neo\) |  |
| ELINF722 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | M1-M4 \(tested elin-neo\) |  |
| EXF722DUAL | [Snippet](https://github.com/betaflight/bidircfg/blob/master/EXF722DUAL-upgrade.cf) |  | M1-M8 |  |
| FLYWOOF7DUAL | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | M1-M6 |  |
| FORTINIF4 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | M1-M4\(tested QuadMcFly\) |  |
| FOXEERF722DUAL | [Snippet](https://github.com/betaflight/bidircfg/blob/master/FOXEERF722DUAL-upgrade.cf) |  | M1-M6 |  |
| FURYF4 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/FURYF4SD-upgrade.cf) |  | M1-M4, No LED support, Tested RawFPV |  |
| FURYF7 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | M1-M4 |  |
| HAKRCF722 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/HAKRCF722-upgrade.cf) |  | M1-M6 |  |
| KAKUTEF4V2 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  |  | M1-M4 tested |
| KISSFCV2F7 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/KISSFCV2F7-upgrade.cf) |  | M1-M6 |  |
| LUXF4OSD | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  |  | M1-M4 tested \(Mister\_M\) |
| MAMBAF411 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | DMA & timer reviewed by joelucid |  |
| MAMBAF722 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/MAMBAF722-upgrade.cf) |  | M1-M4 tested \(kc10kevin\) |  |
| MATEKF405 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | M1-M4 tested \(Wudz\_17\) |  |
| MATEKF411 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | DMA & timer reviewed by joelucid |  |
| MATEKF411RX | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | DMA & timer reviewed by joelucid |  |
| MATEKF722 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | M1-M8 |  |
| MATEKF722SE | [Snippet](https://github.com/betaflight/bidircfg/blob/master/MATEKF722SE-upgrade.cf) | M5不工作 | M1-M4, M6-M8 |  |
| MLTEMPF4 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | M1-M4 \(tested RC2 monko760\) |  |
| NERO | [Snippet](https://github.com/betaflight/bidircfg/blob/master/NERO-upgrade.cf) |  | M1-M8 |  |
| NOX | [Snippet](https://github.com/betaflight/bidircfg/blob/master/NOX-upgrade.cf) |  | M1-M4 |  |
| NUCLEOF7 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) | M4不工作但可使用M6代替 | M1-M3,M6 |  |
| NUCLEOF722 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) | M4不工作但可使用M6代替 | M1-M3,M6 |  |
| OMNIBUSF4 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | M1-M4 \(tested omerco\) |  |
| OMNIBUSF4SD | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | M1-M4 \(tested joe lucid\) |  |
| OMNIBUSF4FW | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | M1-M4 tested \(skonk\) |  |
| OMNIBUSF7 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/OMNIBUSF7-upgrade.cf) |  | M1-M4 \(tested in RC2 IgguT\) |  |
| OMNINXTF7 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | M1-M4 |  |
| PYRODRONEF4 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/PYRODRONEF4-upgrade.cf) |  | M1-M4 \(tested fujin\) |  |
| REVOLTOSD | [Snippet](https://github.com/betaflight/bidircfg/blob/master/REVOLT-upgrade.cf) |  | M1-M4 \(tested JayBird\) |  |
| SPEEDYBEEF4 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/SPEEDYBEEF4-upgrade.cf) |  | Flavoredcrayon |  |
| SPRACINGF7DUAL | [Snippet](https://github.com/betaflight/bidircfg/blob/master/SPRACINGF7DUAL-upgrade.cf) |  | M1-M10 |  |
| TMOTORF4 |  | M4-5不工作但可使用M6代替 | M1-M3, M6 |  |
| YUPIF7 | [Snippet](https://github.com/betaflight/bidircfg/blob/master/DEFAULT-upgrade.cf) |  | M1-M6 |  |

下列是配置代码片段的设置说明

#### 循环更新周期和Dshot协议

双向Dshot可与Dshot150、Dshot300和Dshot600一起使用。出于实际目的，Dshot600在所有PID速率下均可正常工作。

请记住，对于每一个发送的命令帧，现在将有一个帧返回，并且输出帧和返回帧之间有25us的时间来切换通信方向、DMA和计时器。循环更新周期必须选择得足够低，以使得刚好可以在一个命令周期内完成Dshot协议速率下的两个帧+50us。

双向Dshot和RPM滤波器都消耗大量CPU资源，并且这使得环路速率精准到位变得尤为重要，因为只有这样滤波器才能调整到正确的频率上。建议您在首次试飞时使用Dshot600以4k/4k飞行。Dshot600的速率适用于所有循环更新频率。

在F4上，每根电调信号线更换方向需要花费3-4us。所有信号线全部切换两次方向需要花费大约24-23us。RPM滤波器内含36个陷波滤波器，它们以1000hz的频率更新截止频率。

8k/4k在大部分F405上都可以正常工作；可以使用8k/8k但需要超频。F411需要超频才能以8k/4k工作。F7可以使用8k/8k不需要超频。

您可能需要关闭电调自定义启动音乐，因为这可能会干扰双向Dshot。默认启动音工作正常。

#### DMA

旧版双向Dshot需要使用正常的DMA，而不是DMA brust。关闭DMA brust可能并不适用于特定飞控。你可以通过输入以下命令进行简单的尝试：

> set dshot\_burst=off

并检测你的飞机是否还能够进行飞行。如果可以，请继续下一步操作：

#### **启用新的程序调度策略**

由于RPM滤波使用非常窄的陷波滤波器，因此经滤波后的陀螺仪环路时间不会发生变化，完全符合数学期望结果。在曾经，这需要降低环路速率并进行超频。现在已经添加了推程序调度策略的一些更改，即使在更高的环路速率下，也可以实现陀螺仪速率在滤波前后保持一致。双向Dshot需要启用以下命令：

`set scheduler_optimize_rate=on`

#### **启用双向Dshot**

`set dshot_bidir=on`

看看你的电机能否正常旋转。如果可以，请尝试拔下USB线缆，连上电池并重新连接USB。转到CLI，键入status。您应该会看到Dshot的遥测报告。每台电机的RPM读数应该是0，通常这里不会发生错误。

#### **电机磁极数**

ESC报告eRPM，需要使用电机的磁极数以将其转换为RPM。磁极数可以在电机的定子外壳上找得到，不要数绕组所在的定子磁铁数量。典型的5寸电机有14个电机，因此14是默认值。较小的电机通常具有较少的磁极数，通常为12个。计数或查看电机规格以知悉确切磁极数量，并使用如下命令以进行配置：

`set motor_poles=14`

#### **验证环路时间一致**

**重要：**

启用上述所有功能后，请仔细检查您的环路速率是否一致。如果不一致，请选择较低的环路速率。请记住，与滤波不同，环路速率对飞行性能的影响非常小。

为此，请在CLI中输入tasks并检查陀螺仪环路速率与您指定的环路速率是否相符（注意，电池应连接飞控已获得准确的环路速率结果）。例如：

```text
# tasks
Task list             rate/hz  max/us  avg/us maxload avgload     total/ms
00 - (         SYSTEM)     10       1       0    0.5%    0.0%         0
01 - (         SYSTEM)   1000       3       1    0.8%    0.6%       522
02 - (       GYRO/PID)   7999      43      34   34.8%   27.6%      2845
03 - (            ACC)   1000      12      10    1.7%    1.5%       107
04 - (       ATTITUDE)    100      17      10    0.6%    0.6%        11
05 - (             RX)     32      34      32    0.6%    0.6%        12
06 - (         SERIAL)    100     851       3    9.0%    0.5%         8
08 - (BATTERY_VOLTAGE)     50       4       2    0.5%    0.5%         1
09 - (BATTERY_CURRENT)     50       1       1    0.5%    0.5%         0
10 - ( BATTERY_ALERTS)      5       3       2    0.5%    0.5%         0
11 - (         BEEPER)    100       2       1    0.5%    0.5%         1
14 - (           BARO)     43      98      66    0.9%    0.7%        34
15 - (       ALTITUDE)     40       7       3    0.5%    0.5%         1
17 - (      TELEMETRY)    250       1       0    0.5%    0.0%        27
19 - (            OSD)     60      21      13    0.6%    0.5%         9
21 - (            CMS)     60       1       1    0.5%    0.5%         0
22 - (        VTXCTRL)      5       1       1    0.5%    0.5%         0
23 - (        CAMCTRL)      5       1       1    0.5%    0.5%         0
25 - (    ADCINTERNAL)      2       3       1    0.5%    0.5%         0
26 - (       PINIOBOX)     19       1       1    0.5%    0.5%         0
RX Check Function                   2       1                         0
Total (excluding SERIAL)                        46.0%   37.1%
```

您需要检查GYRO/PID这一行：

```text
02 - (       GYRO/PID)   7999      43      34   34.8%   27.6%      2845
```

在这种情况下，我们已将GYRO/PID环路速率配置成为8k/8k，此行显示它正运行着7999hz的环路速率。它必须非常接近8k（8000Hz）。建议将误差保持在1%以下。

#### 调试模式

有两种黑盒调试模式可以用来验证RPM滤波器：`RPM_FILTER`记录电调报告的每个电机的频率。`DSHOT_RPM_TELEMETRY`记录未经转换的eRPM。

#### TPA

此代码将应用4.1默认TPA配置（1250起始，仅衰减D至满油门下65%）。

```text
set tpa_rate = 65
set tpa_breakpoint = 1250
```

### 参考来源

[双向Dshot PR](https://github.com/betaflight/betaflight/pull/7264)

[RPM滤波器PR](https://github.com/betaflight/betaflight/pull/7271)

