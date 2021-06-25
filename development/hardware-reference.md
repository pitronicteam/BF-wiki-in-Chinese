# 硬件参考

### 简介

该页面为硬件开发者提供了未来的飞控板的细节，以确保与Betaflight的最大兼容性。

### 目标维护

硬件开发人员负责在Betaflight中开发和维护他们的目标。目标文件应该尽可能地与主代码相互分开，以便于实现易维护性。

### 添加新目标

如果您要添加新的飞控目标，那么：

1. 对`master`分支提交PR
2. 不要更改`travis.yml`或`fake_travis_build.sh`文件——这些只是用来检查PR并尝试编译固件的一个子集
3. 向wiki添加一个描述页面，用以描述飞控板并提供至少一个供应商的链接。

### 硬件

#### MPU（SPI和I2C）

#### MPU中断

#### 黑盒芯片

#### 单片机

数据手册/参考手册的摘录，涵盖了可能的引脚、定时器、DMA分配：

* [STM32F3](https://github.com/betaflight/betaflight/wiki/reference/stm/stm32f3_pins_timers_dma.pdf)
* [STM32F405](https://github.com/betaflight/betaflight/wiki/reference/stm/stm32f405_pins_timers_dma.pdf)
* [STM32F411](https://github.com/betaflight/betaflight/wiki/reference/stm/stm32f411_pins_timers_dma.pdf)
* [STM32F722](https://github.com/betaflight/betaflight/wiki/reference/stm/stm32f722_pins_timers_dma.pdf)
* [STM32F745](https://github.com/betaflight/betaflight/wiki/reference/stm/stm32f745_pins_timers_dma.pdf)
* [STM32H743](https://github.com/betaflight/betaflight/wiki/reference/stm/stm32h743_pins.pdf)（仅含引脚）

### 协议

#### 遥测

[IBus 遥测规范](https://github.com/betaflight/betaflight/wiki/Ibus-telemetry)

、

