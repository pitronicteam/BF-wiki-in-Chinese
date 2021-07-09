# 气压计配置

### 介绍

在3.2中，气压计驱动被转换为完全可配置/可重新配置的形式。

这样做的副作用便是，target.h中相关宏定义的解释方式发生了很大的变化，我们已经采取了相关措施保证其向后兼容性，但在某些情况下，某些使用气压计的配置可能无法再3.2上正常使用。

此页面解释了使气压计恢复到正常工作状态的相关的CLI变量/命令，以及飞控目标维护者所需的默认配置的更改。

### 使用CLI配置气压计

* 与气压计相关的CLI变量

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53D8;&#x91CF;</th>
      <th style="text-align:left"><code>&#x8303;&#x56F4;</code>
      </th>
      <th style="text-align:left"><code>&#x63CF;&#x8FF0;</code>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>baro_bustype</code>
      </td>
      <td style="text-align:left"><code>NONE</code>,<code>I2C</code>,<code>SPI</code>
      </td>
      <td style="text-align:left">&#x6307;&#x5B9A;&#x6240;&#x9009;&#x6C14;&#x538B;&#x8BA1;&#x8BBE;&#x5907;&#x7684;&#x603B;&#x7EBF;&#x7C7B;&#x578B;</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>baro_i2c_device</code>
      </td>
      <td style="text-align:left"><code>0</code>~MCU&#x7684;&#x6700;&#x5927;I2C&#x603B;&#x7EBF;&#x5730;&#x5740;&#xFF08;&#x8D77;&#x59CB;&#x4E3A;<code>1</code>&#xFF0C;&#x4E0E;<code>target.h</code>&#x4E2D;&#x7684;<code>I2Cx</code>&#x4E2D;&#x7684;<code>x</code>&#x5B9A;&#x4E49;&#x76F8;&#x540C;&#xFF09;</td>
      <td
      style="text-align:left">&#x6307;&#x5B9A;&#x8BE5;&#x8BBE;&#x5907;&#x5728;I2C&#x603B;&#x7EBF;&#x4E0A;&#x7684;&#x5750;&#x6807;&#x3002;&#x4EC5;&#x5F53;<code>baro_bustype</code>&#x4E3A;<code>I2C</code>&#x65F6;&#x6709;&#x6548;&#x3002;<code>0</code>&#x8868;&#x793A;&#x201C;&#x65E0;&#x201D;</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>_i2c_address</code>
      </td>
      <td style="text-align:left"><code>0</code>~<code>199</code>(0x77)</td>
      <td style="text-align:left">&#x8BBE;&#x5907;&#x7684;&#x4E03;&#x4F4D;I2C&#x5730;&#x5740;&#x503C;&#x3002;<code>0</code>&#x662F;&#x4E00;&#x4E2A;&#x7279;&#x6B8A;&#x503C;&#xFF0C;&#x7528;&#x4E8E;&#x6307;&#x5B9A;&#x201C;<em>&#x9A71;&#x52A8;&#x8BBE;&#x5907;&#x9ED8;&#x8BA4;&#x5730;&#x5740;</em>&#x201D;&#x3002;&#x5F53;&#x503C;&#x4E3A;<code>1</code>~<code>7</code>&#x65F6;&#x5730;&#x5740;&#x65E0;&#x6548;&#x4E14;&#x884C;&#x4E3A;&#x4E0D;&#x53EF;&#x9884;&#x6D4B;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>baro_spi_device</code>
      </td>
      <td style="text-align:left"><code>0</code>~MCU&#x7684;&#x6700;&#x5927;SPI&#x603B;&#x7EBF;&#x5730;&#x5740;&#xFF08;&#x8D77;&#x59CB;&#x4E3A;<code>1</code>&#xFF0C;&#x4E0E;<code>target.h</code>&#x4E2D;&#x7684;<code>SPIx</code>&#x4E2D;&#x7684;<code>x</code>&#x5B9A;&#x4E49;&#x76F8;&#x540C;&#xFF09;</td>
      <td
      style="text-align:left">&#x6307;&#x5B9A;&#x8BE5;&#x8BBE;&#x5907;&#x5728;SPI&#x603B;&#x7EBF;&#x4E0A;&#x7684;&#x5750;&#x6807;&#x3002;&#x4EC5;&#x5F53;<code>baro_bustype</code>&#x4E3A;<code>SPI</code>&#x65F6;&#x6709;&#x6548;&#x3002;<code>0</code>&#x8868;&#x793A;&#x201C;&#x65E0;&#x201D;</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>baro_hardware</code>
      </td>
      <td style="text-align:left"><code>NONE</code>,<code>AUTO</code>,<code>BMP280</code>,<code>MS5611</code>,<code>BMP055</code>
      </td>
      <td style="text-align:left">
        <p>NONE=&#x7981;&#x7528;&#x6C14;&#x538B;&#x8BA1;&#x652F;&#x6301;</p>
        <p>AUTO=&#x56FA;&#x4EF6;&#x5C06;&#x6839;&#x636E;&#x9884;&#x5B9A;&#x4E49;&#x7684;&#x89C4;&#x5219;&#x6765;&#x786E;&#x5B9A;&#x6C14;&#x538B;&#x8BA1;&#x8BBE;&#x5907;&#x3002;</p>
        <p>BMP280&#xFF0C;MS5611&amp;BMP085=&#x660E;&#x786E;&#x4F7F;&#x7528;&#x6307;&#x5B9A;&#x7684;&#x8BBE;&#x5907;&#x9A71;&#x52A8;</p>
      </td>
    </tr>
  </tbody>
</table>

* 如果设备是通过SPI连接的，则可以使用CLI命令resource来指定CS（片选）引脚

| 资源名称（resource name） | 描述 |
| :--- | :--- |
| `BARO_CS` | 指定用于开启SPI总线上的气压计的CS（片选）引脚 |

* 请注意，并非所有总线类型和设备的组合都是针对某个特定飞控目标而内置好的
* 如果使用`baro_hardware = AUTO`，`baro_bustype = I2C`和`baro_i2c_address = 0`的组合将会使得飞控在I2C类型气压计设备上按照`BMP280`，`MS5611`和`BMP085`的顺序循环扫描`baro_i2c_device`。



