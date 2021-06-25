---
description: 使用resource命令重新映射电机（3.1+）
---

# 重新映射电机

有时我们不得不交换/移动/旋转电机布局。在3.1之前，我们必须在飞控板和电调之间使用长导线以保留原始映射（但这会造成飞机布局混乱），或使用自定义mmix更改每个电机对姿态调整的贡献逻辑。

在Betaflight 3.1+版本中，我们可以使用CLI命令resource来轻松映射电机。有关此命令的说明，请参见Betaflight资源重映射。

视频： 

Joshua Bardwell视频：[Resource Remapping- No more Custom Motor Mixer](https://www.youtube.com/watch?v=z5aO-3_n-Hs)

Project Blue Falcon视频：[Find Bad ESC output Pin and Remap Motors](https://www.youtube.com/watch?v=cEMfs_4X2VM)

另一个Blue Falcon视频：[Remap Motors In Betaflight \(damaged pins fix\)](https://www.youtube.com/watch?v=cEMfs_4X2VM&feature=youtu.be&t=159)

### 使用样例：

这里以最常见的“旋转飞控板或旋转分电板”为例。

下设您有一块电机编号如下所示的分电板：

     `FRONT  
4               2  
  
3               1  
     BACK`

出于某种原因，您想将电路板顺时针旋转90度，让它最后变成下面这个样子

     `FRONT  
3               4  
  
1               2  
     BACK`

让我们来一起解决这个问题吧。

1. 记录当前的电机映射 在CLI中输入resource list以查看原始映射 `# resource list ... A06: MOTOR 1 A07: MOTOR 2 A11: MOTOR 3 A12: MOTOR 4 ...`
2. 画一个带有原始引脚到电机的映射关系的图表      `FRONT 4(A12)      2(A7)  3(A11)      1(A6)      BACK`
3. 将这个图顺时针旋转90度。      `FRONT 3(A11)      4(A12)  1(A6)       2(A7)      BACK`
4. 移除旧的电机标号  
        `FRONT  
   A11         A12  
  
   A6          A7  
        BACK`

   这是您实际的MCU引脚-电机位置映射关系

5. 将新的电机位置标号分配给MCU引脚  
        `FRONT  
   4(A11)      2(A12)  
  
   3(A6)       1(A7)  
        BACK`

   这是您的新映射。

6. 用于此映射的CLI resource命令为： `resource motor 1 a7 resource motor 2 a12 resource motor 3 a6 resource motor 4 a11 save` 输入这些命令时，您将看到一些错误提示消息。映射重叠是过渡性的，最终您将得到一份干净的映射表。如果您不喜欢看到这些红色的错误消息，则可以在输入新映射之前，先输入一下命令来清除旧映射： `resource motor 1 none resource motor 2 none resource motor 3 none resource motor 4 none`

这只是一个简单的例子。请您自行处理您的实际问题，并在飞行之前验证新映射是否正确。

祝您玩的开心！！！

