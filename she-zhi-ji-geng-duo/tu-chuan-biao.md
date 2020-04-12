# 图传表

### 在配置内程序内使用的图传表

使用说明：

* 右键单击“另存为”；
* 在配置器中，转到“图传”页面，使用“从文件加载”加载在上一步中保存的文件；
* **单击“保存”**以将图传表保存在飞控上。

有关如何快速确定SmartAudio版本的视频，请访问：https://youtu.be/eaSmoOPk9KY?t=65

SmartAudio的Debug\[0\]通道：

* 100 = SA 1.0
* 116 = SA 1.0 解锁
* 200 = SA 2.0
* 216 = SA 2.0 解锁
* 300 = SA 2.1
* 316 = SA 2.1 解锁



| 制造商/型号配套文件 | 文件 |  |
| :--- | :--- | :--- |
| **IRC Tramp协议：** |  |  |
| IRC Tramp | [IRC Tramp \(USA\)](resources/vtx_tables/vtx_table_irc_tramp_us.json) |  |
|  | [IRC Tramp \(EU\)](resources/vtx_tables/vtx_table_irc_tramp_eu.json) |  |
| [MATEKSYS VTX-MINI](http://www.mateksys.com/?portfolio=vtx-mini#tab-id-6) | [VTX-MINI \(INT\)](http://www.mateksys.com//Downloads/VTX/MATEK_VTX-mini.json) |  |
| [iFlight The Force Long Range](https://www.iflight-rc.com/index.php?route=product/product&path=24_75&product_id=732) | [Force LR \(USA\)](https://raw.githubusercontent.com/Maizzer/Betaflight-VTX-Tables/master/BTFL_vtxtable_iFlight_Force_Long_Range_-_US.json) |  |
| [RunCam TX200U](https://shop.runcam.com/runcam-tx200u/) | [IRC Tramp \(USA\)](https://runcamfcfiles.s3-us-west-2.amazonaws.com/vtxtable/betaflight/TX200U/runcam_tx200u_vtx_table_irc_tramp_us.json) |  |
|  | [IRC Tramp \(EU\)](https://runcamfcfiles.s3-us-west-2.amazonaws.com/vtxtable/betaflight/TX200U/runcam_tx200u_vtx_table_irc_tramp_eu.json) |  |
| [RunCam TX100](https://shop.runcam.com/runcam-tx100-nano/) | [IRC Tramp \(USA\)](https://runcamfcfiles.s3-us-west-2.amazonaws.com/vtxtable/betaflight/TX100/runcam_tx100_vtx_table_irc_tramp_us.json) |  |
|  | [IRC Tramp \(EU\)](https://runcamfcfiles.s3-us-west-2.amazonaws.com/vtxtable/betaflight/TX100/runcam_tx100_vtx_table_irc_tramp_eu.json) |  |
| [Speedy Bee TX500](https://www.speedybee.com/tx500/) | [IRC Tramp \(USA\)](https://speedybee.s3.amazonaws.com/vtxtable/betaflight/TX500/speedybee_tx500_vtx_table_irc_tramp_us.json) |  |
|  | [IRC Tramp \(EU\)](https://speedybee.s3.amazonaws.com/vtxtable/betaflight/TX500/speedybee_tx500_vtx_table_irc_tramp_eu.json) |  |
|  |  |  |
| **TBS SmartAudio协议：** |  |  |
| TBS（SA 1.0仅适用于第一代TBS图传） | [SmartAudio 1.0 \(USA\)](resources/vtx_tables/vtx_table_smart_audio_1_0_us.json) |  |
|  | [SmartAudio 1.0 \(EU\)](resources/vtx_tables/vtx_table_smart_audio_1_0_eu.json) |  |
| TBS（适用于大多数支持SmartAudio的图传） | [SmartAudio 2.0 \(USA\)](resources/vtx_tables/vtx_table_smart_audio_2_0_us.json) |  |
|  | [SmartAudio 2.0 \(EU\)](resources/vtx_tables/vtx_table_smart_audio_2_0_eu.json) |  |
| TBS（当前仅适用于最新的TBS图传，例如EVO或Pro32） | [SmartAudio 2.1 \(USA\)](resources/vtx_tables/vtx_table_smart_audio_2_1_us.json) |  |
|  | [SmartAudio 2.1 \(EU\)](resources/vtx_tables/vtx_table_smart_audio_2_1_eu.json) |  |
|  |  |  |
| **板载图传:** |  |  |
| 板载图传的飞控板 | [RTC6705 \(USA\)](resources/vtx_tables/vtx_table_rtc6705_us.json) |  |
|  | [RTC6705 \(EU\)](resources/vtx_tables/vtx_table_rtc6705_eu.json) |  |

### 在CLI中设置图传表

请查阅[这个](https://github.com/betaflight/betaflight/blob/master/docs/VTX.md#vtx-table)文档。

### 图传按钮用法

按住图传按钮时，状态2 LED每秒将闪烁N次，用以表示当您释放按钮时将采取的操作。按住按钮后，闪烁开始。例如，您按下按钮，然后您记录闪烁次数，然后根据需要释放。

```text
| Duration      | Function                  | Flashes   |
|---------------|---------------------------|-----------|
```

重启图传的示例：

```text
| 0 seconds     | 1 second     | 2 seconds    | 3 seconds    | 4 seconds    | 5 seconds    | 6 seconds or more|
[-HOLD BUTTON------------------------------|-RELEASE BUTTON-NOW------------|-RELEASED TO LATE TO CHANGE POWER |
| 4 Flashes     | 3 flashes    | 3 flashes   | 2 flashes    | 2 flashes    | 1 flash      | 1 flash           |
| 0 seconds     | 1 second     | 2 seconds   | 3 seconds    | 4 seconds    | 5 seconds    | 6 seconds or more |
|-HOLD BUTTON------------------------------|-RELEASE BUTTON-NOW------------|-RELEASED TOO LATE TO CHANGE POWER|
| 4 Flashes     | 3 flashes    | 3 flashes   | 2 flashes    | 2 flashes    | 1 flash      | 1 flash           |
```

图传按钮可以与所有图传系统配合使用，包括板载RTC6705，Tramp和SmartAudio。

如果可以关闭图传，那么Power 0将关闭图传，而Power 1将把图传设置为其最低输出功率。如果无法关闭图传，那么Power 0将把图传设置为其最低输出功率。

一些图传其自身有一定的使用限制。例如，SmartAudio 1.0和2.0设备只能在初次上电时进入pitmode。Betaflight可以使这些设备退出pitmode，但无法控制其进入。

