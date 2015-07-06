---
date: 2015-06-13T17:30:00+08:00
title: "负反馈放大电路"
---

明天将要进行《模拟电子线路》课程的期末考试。今日特地记一道题目以作纪念。

问题是这样的：反馈放大电路如图所示。请

1. 判断反馈的组态；
2. 估算闭环增益\\(A\_f\\)。

已知：\\(R\_s\\)的值已经很高，可以略去。

{{% figure src="/media/circuit-1.png" %}}

<!--more-->

首先，根据“输出短路法”，将\\(v\_o\\)置零时，发现反馈回路消失，由此可知该电路为电压反馈。其次，反馈回路和输入线路并接在同一端，由此可知该电路为并联反馈。最后，根据“瞬时极性法”，可以判断反馈信号和输入信号是反相的，由此可知该电路为负反馈。

对于电压并联负反馈而言，闭环增益为互阻增益\\(A\_{rf}={v\_o\over i\_i}\\)。

对于负反馈放大电路，闭环增益\\(A\_f={A\over1+AF}\\)。为了估算闭环增益，假设该电路满足深度负反馈的条件：反馈深度\\(1+AF\\)在数量上非常大，忽略其中的常数1，得\\(A\_f\approx{1\over F}\\)。
也就是\\[{v\_o\over i\_i} \approx {v\_o \over i\_f}\\]

事实上，题中给出的条件\\(R\_s\\)的值很高，意义在于：信号源可以等效为一恒流源和内阻\\(R\_s\\)的并联结构。\\(R\_s\\)越小，则反馈电流越可能经过\\(R\_s\\)流到地，如果信号源内阻为0，则反馈也就不存在了。\\(R\_s\\)的值很高，可以视作断路，那么反馈电流全部流入T1的基极，反馈的效果好，所以容易实现深度负反馈。由此可知我们假设该电路满足深度负反馈是有道理的。

注意到深度负反馈时，可以得到\\(i\_i\approx i\_f\\)。因此，反馈放大电路的净输入电流\\(i\_{id}\\)（即三极管T1的基极电流）近似等于0。此现象称作深度负反馈的“虚断”。从输入端口看进去，基本放大电路可以看作一个输入电阻。而在净输入电流为0的情况下，输入电阻上的电压也就为0，因此输入端口（T1的基极到地）上的电压是0。此现象称作深度负反馈的“虚短”。

注意：本文所讨论的电压和电流，指的都是交流通路中的交流量。

知道基极电位以后，则\\(R\_{f2}, R\_{e4}, R'\_{e4}\\)的状态就都可以确定了。不难算出\\(R\_{f2}\\)上流过的电流
\\[i\_f = -{1\over R\_{f2}} {R'\_{e4} \\| R\_{f2} \over R\_{e4} + (R'\_{e4} \\| R\_{f2})}v\_o\\]

做一些代数运算可得\\[A\_f \approx {v\_o\over i\_f} = -{R\_{e4}(R'\_{e4}+R\_{f2}) + R'\_{e4}R\_{f2} \over R'\_{e4}}\\]

这题还有一种奇怪的解法。那就是利用上面所说的“虚断”和“虚短”条件，把基本放大电路接成运算放大器，如下

{{% figure src="/media/circuit-2.png" %}}

根据“瞬时极性法”，输出信号和输入信号是反相的。所以输入信号接在运算放大器的反相输入端。画出这个电路以后，使用运算放大器的知识不难求得答案。

## 后记

我一直在寻找一个绘制电路图的工具。不过，至今为止尚未找到满意的。本文中的电路图是用[VectorBlocks](http://bretmulvey.com/VectorBlocks/)绘制的。这个工具的特点是符号比较齐全，画出来的电路图比较漂亮（相对于LTSpice的界面而言）。但是操作不太方便，绘制电路图耗时比较多。我想，如果能改进用户界面，并且具备自动布线功能的话，就完全符合我的需求了。