# 目录

* [32. PORT - I/O口控制器](#32-PORT - I/O口控制器)



# 31.事件系统
## 31.1 综述
   <br/>事件系统（EVSYS）允许外围设备之间的自主，低延迟和可配置通信。
   <br/>可以配置若干外围设备以生成和/或响应称为事件的信号。 生成事件的准确条件或接收事件时采取的操作特定于每个外围设备。
   <br/>响应事件的外围设备称为事件用户。 生成事件的外围设备称为事件生成器。 外围设备可以具有一个或多个事件生成器，并且可以具有一个或多个事件用户。
   <br/>无需CPU干预即可进行通信，无需消耗总线或RAM带宽等系统资源。 与传统的基于中断的系统相比，这减少了CPU和其他系统资源的负载。


## 31.3 模块框图
![image](https://github.com/yuchengstudio/SAMD51/blob/master/aplication_note/pictures/Event%20System_001.png)

## 31.5 功能描述
   <br/>事件系统由几个通道组成，这些通道从外围设备路由内部事件（发生器）到其他内部外围设备或I / O引脚（用户）。 可以选择每个事件生成器作为多个通道的源，但不能将通道设置为同时使用多个事件生成器
   <br/>可以以异步，同步或重新同步的操作模式配置通道路径。必须根据应用程序的要求选择操作模式。
   <br/>使用同步或重新同步路径时，事件系统包括包括如下选项:当检测到事件发生器的上升沿，下降沿，或是双边沿将事件传递给用户.
   
#### 31.5.2.6 通道路径
从事件生成器传播事件有不同的方法：
<br/>异步路径
<br/>同步路径
<br/>再同步路径
<br/>通过写入通道寄存器的路径选择位组（CHANNELn.PATH）来决定路径

<br/>异步路径
<br>使用异步路径时，事件将从事件生成器传播到事件用户无需事件系统的干预。 此通道的GCLK（GCLK_EVSYS_CHANNEL_n）不是必需的，这意味着事件将传播给用户而没有任何时钟延迟
<br>选择异步路径时，通道不能生成任何中断，并且通道x状态寄存器（CHSTATUSx）始终为零。 边缘检测不是必需的，必须禁用软件。 每个外围事件用户必须选择哪个事件边缘必须触发内部操作。


<br/>同步路径
<br/>当事情发生器和事件通道使用通用时钟的相同时钟发生器时，应选用同步路径。如果它们不使用相同的时钟，则可能无法在通道中检测到从事件生成器到事件通道的逻辑更改，这意味着该事件将不会传播到事件用户。
<br/>当使用同步路径，通道能够产生中断。在通道状态寄存器（CHSTATUS）中断状态位可以更新，和获得。

<br/>重新同步路径
<br/>当事情发生器和事件通道没有使用通用时钟的相同时钟发生器时，应选用重新路径，当重新同步路径使用，事件生成器中事件的重新同步是在通道中完成的。
<br/>使用重新同步的路径时，通道可以生成中断。 通道状态位通道状态寄存器（CHSTATUS）中的信息也已更新并可供使用。



   
# 32. PORT - I/O口控制器
## 32.1 综述
   IO引脚控制器（PORT）控制器件的I / O引脚。 I / O引脚被组织在一系列组中，统称为PORT组。 每个端口组最多可以有32个引脚
这32个引脚可独立配置，控制，或以组的方式配置，控制。 设备上的端口组数量取决于封装/引脚数量。 每个引脚可用于通用GPIO被应用直接控制
或分配给嵌入式设备外设。当用于通用I / O时，每个引脚都可以配置为输入或输出，具有高度可配置的驱动器和上拉设置。

   当用作GPIO口时，所有的I/O口都具有真正的读取 - 修改 - 写入功能; 方向还是一个或多个引脚的输出值可能会改变（设置，
复位或切换）无意中更改同一端口组中任何其他引脚的状态由单个原子的8，16或1632位写入。
   PORT通过AHB / APB桥连接到高速总线矩阵
   
## 32.2 功能
每个引脚的可选输入和输出配置
•I / O引脚上的外设功能的软件控制复用
•通过专用引脚配置寄存器进行灵活的引脚配置
•可配置的输出驱动器和拉式设置：
- 图腾柱（推挽）
- 拉配置
- 司机的实力
•可配置的输入缓冲区和拉式设置：
- 内部上拉或下拉
- 输入抽样标准
- 如果不需要，可以禁用输入缓冲器以降低功耗
•输入事件：
- 每个端口组最多有四个输入事件引脚
- SET / CLEAR / TOGGLE事件动作，用于输入引脚输出值的每个事件
- 可以输出到引脚
•使用STANDBY模式节电
- 无法访问配置寄存器
- 可能访问数据寄存器（DIR，OUT或IN）


## 32.3 框图
![images](https://github.com/yuchengstudio/SAMD51/blob/master/aplication_note/pictures/E5X_DT_001.jpg)

## 32.4 信号描述
表格32-1. PORT的信号描述
![images](https://github.com/yuchengstudio/SAMD51/blob/master/aplication_note/pictures/E5X_DT_002.jpg)
有关此外设的引脚映射的详细信息，请参阅I / O复用和注意事项(I/O Multiplexing and Considerations)。
一个信号可以映射到多个引脚。

## 32.5 产品相关性
为了使用这个外设，系统的其他部分必须正确配置如下。

### 32.5.1. I/O口线
PORT的I / O线被映射到物理设备的引脚。 使用以下命名规则：

  单个的I/O口线组合成成含32个口线的线束组，并被分配一个标识符'xy'，字母x = A，B，C ...和两位数编号y = 00,01，... 31。 
  例子：A24，C03。
  PORT引脚相应标记为“Pxy”，例如PA24，PC03。 这标识了设备中的每个引脚唯一。
  
  每个引脚都可以通过一个或多个外设多路复用器设置进行控制，这可以使焊盘能内部链接到专有的外设功能。 当设置被启用时，所选的外设具有可以控制焊盘的输出状态，以及读取当前物理焊盘状态的能力。
  有关详细信息，请参阅I / O复用和注意事项（I/O Multiplexing and Considerations）。
  设备特定的配置可能会导致某些I/O线功能（和相应的Pxy引脚）不可实现。
  
### 32.5.2. 功耗管理
   复位期间，所有端口线都配置为输入功能，具有输入缓冲区，输出缓冲区，同时禁用上拉。
当器件设置为BACKUP睡眠模式时，即使PORT配置寄存器和输入同步，将丢失它们的内容（当PORT再次通电时这些内容不会被恢复），
引脚的锁存器将保持其当前配置，例如输出值和上拉设置。 参考到Power Manager文档以获取更多与I / O线路配置相关的功能
备份模式。
   PORT外设将继续在其源时钟运行的任何睡眠模式下运行。
   
### 32.5.3. 时钟
   可以在主时钟模块中启用和禁用端口总线时钟（CLK_PORT_APB），然后使用可以在MCLK - Main中的外设时钟屏蔽部分找到CLK_PORT_APB的默认状态时钟。
PORT需要一个APB时钟，它是CPU主时钟的一个分频时钟，并允许CPU
通过高速矩阵和AHB / APB桥访问PORT的寄存器。
如果并发端口访问，EVSYS和APB将插入等待状态。
PORT输入同步器使用CPU主时钟，以使重新同步延迟最小化
关于APB时钟。


```
待解决的问题
如何开启内部“DRVSTR”功能，并检测时钟输出是否能整形成功。
```

# 54. 电器参数
### 54.13.2 SPI时钟特性
![images](https://github.com/yuchengstudio/SAMD51/blob/master/aplication_note/pictures/E5x_SPI_001.jpg)











