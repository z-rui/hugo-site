---
date: 2016-10-14T22:08:00+08:00
title: PIC微控制器
---

PIC是[Microchip](http://www.microchip.com/)公司生产的微控制器，有8位、16位、32位的型号。其中PIC12系列为8引脚8位微控制器，它的引脚数非常少，很适合做一些小型项目。例如最简单的[流水灯]({{<relref "led_circuit.md" >}})，如果用微控制器来做，那么就不需要其他IC芯片了（振荡器为微控制器自带，而移位寄存器的功能可以用软件实现）。

我有一片PIC12F629和一片PIC12F675。两者功能基本一致，后者多了A/D功能。之前我尝试自制PIC编程器给629编程，试了很久也没有成功，只好买了一个正规的编程器来用，结果发现629似乎被我折腾坏了。现在只剩下一片675，我用它制作了两个小项目：音乐播放器和字符显示器。

{{<figure src="/media/picmcu-1.jpg" >}}
图中：上为字符显示器，下为音乐播放器。音乐播放器中的PIC芯片被拔下来了。

<!--more-->

## 音乐播放器

{{<figure src="/media/picmcu-2.png" >}}

这里我用了一个额外的555定时器产生节拍时钟。这个时钟控制音乐的节拍，改变电位器的值就可以改变音乐播放的速度。生成各个音的频率则使用PIC12F675内置的定时器。

在PIC12F675内部，有两个定时器Timer0和Timer1，它们各自可以设置一个预分频器(prescaler)。这个功能很有用，例如本来是四分音符的时长为一拍，经过一级分频之后，就变成了二分音符的时长；对于音符而言，经过一级分频，频率降低一半，即降低一个八度。

软件只需要实现最短的音符（我实现的是十六分音符）和最高的八度（我是实现的是音C7所在的八度），此外只需要调整预分频器的参数即可，非常方便。

原理图中的蜂鸣器部分有所简化，实物中是一个由PNP管控制的蜂鸣器模块，主要是为了方便那些驱动能力不够的芯片。据我所知PIC12系列的IO引脚驱动能力有20mA，可以直接驱动蜂鸣器。调节和蜂鸣器串联的可调电阻可以控制音量。当然，如果用对数可调电阻，调节起来会显得比较“线性”。

我也许会在以后将软件提交到我的GitHub代码仓库。

这个电路在使用直流电源的时候，并联47μF的电容(也许可以更小，但我试过4.7μF，效果不好)可以有效防止干扰。当不并联电容时，蜂鸣器里只播出杂音而没有音乐。使用电池供电则没有这个问题。

但是，在使用编程器给PIC芯片编程时，则需要去掉并联的电容，否则容易导致编程失败。

## 字符显示器

{{<figure src="/media/picmcu-3.png" >}}

显示器是一块8×8的LED点阵。需要接16个引脚并使用扫描控制，不过PIC12F675只有6个IO引脚所以不可行。可以用74595这样的串入并出寄存器来减少IO口，但是扫描的逻辑还是需要由单片机完成。我这里用了一个现成的基于MAX7219的模块。MAX7219的功能很强，自带扫描和解码的逻辑，而且有驱动能力，可以驱动数码管、LED点阵等。微控制器通过类似SPI的串行通信将需要显示的内容发送到MAX7219，后者就可以生成驱动信号。用MAX7219做成的模块可以串接，不论串接多少块显示器，都只需要3根数据线。

因为显示器的驱动由MAX7219完成，所以PIC中的程序就只剩下了最基本的逻辑、和MAX7219的通信，以及字符的点阵数据。

PIC有一套自己的汇编语言，指令集比较精简。和MAX7219通信的一部分代码如下。

```
; 发送一个字节
TX_BYTE
    MOVLW D'8'
    MOVWF TX_LEN	; TX_LEN初始化为8
; 发送TX_LEN个比特
TX_BITS
    BCF GPIO, MOSI	; MOSI置零
    RLF TX_DATA, F	; 将TX_DATA最高位取出
    BTFSC STATUS, C	; 如果最高位是1
    BSF GPIO, MOSI	;  则MOSI置一 (否则MOSI保持置零)
    BSF GPIO, SCLK	; SCLK置零
    BCF GPIO, SCLK	; SCLK置一，完成一个比特的写入
    DECFSZ TX_LEN, F	; TX_LEN减一
    GOTO TX_BITS	; 若TX_LEN不为零则回到TX_BITS
    RETURN
```

PIC的架构对编译器不够友好，市面上针对PIC架构的C语言编译器很少，主要是Microchip自家的MPLAB XC编译器。上面的程序也可以用C语言写，大概可以写成如下的样子(仅供意会)：

```
void TX_BYTE(uint8 TX_DATA)
{
	uint8 TX_LEN = 8;

	do {
		MOSI = TX_DATA & 0x80;
		TX_DATA <<= 1;
		SCLK = 0;
		SCLK = 1;
	} while (--TXLEN);
}
```

不过对于这种低资源的芯片以及简单的项目而言，倒也没有必要使用C语言。使用汇编编写程序对我来说倒不是什么困难事，问题在于程序没有可移植性，如果需要移植到其他硬件则会是很大的麻烦。话说回来，即便是C程序，涉及到的硬件专有特性（特殊端口名称等）也是不可移植的。所以微控制器的程序移植似乎是一个问题。
