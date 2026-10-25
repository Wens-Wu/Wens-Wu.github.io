---
title: CGMF-Physics Models
date: 2026-10-26 01:33:12
tags:
---

### Fission Fragment Yields

CGMF并不计算初始裂变碎片的产额，其基于实验数据/系统规律，得到初始裂变碎片的质量、电荷、动能等。当前CGMF版本只考虑了二元裂变，也不包含更复杂的裂变过程。

**Mass Yields(质量)**

默认情况下，CGMF采用三高斯模型来表示初始裂变碎片的质量分布。其结构如下：
$$
Y(A;E_N)=G_0(A)+G_1(A)+G_2(A)
$$
**Charge Yields(电荷)**

根据Wahl’s systematics [(Wahl:2002)](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwjB9NTGxqXsAhV-IDQIHZQ9AoAQFjAAegQIBRAC&url=https%3A%2F%2Fwww.osti.gov%2Fservlets%2Fpurl%2F809574&usg=AOvVaw22bd9FvugI6cK369QWoqS8)，电荷分布随碎片质量和入射中子能量的函数遵循高斯形式。这些均值由未改变的电荷分布（UCD）经过观测电荷极化的校正给出。宽度是碎片质量的函数，并根据现有实验数据随能量拟合。使用Wahl’s systematics得出给定质量的电荷分布：
$$
P(Z|A)=\frac{1}{2}F(A)N(A)[erf(V)-erf(W)]
$$
**TKE Distribution(总动能分布)**

平均总动能 $\overline{TKE}$ 是一个重要量，它很大程度上决定了裂变系统中用于发射中子和光子的总激发能。由于绝大多数中子在光子发射之前就被释放，因此平均中子多重性 $\bar{\nu}$ 的准确性很大程度上取决于 $\overline{TKE}$。

**TXE**

两个碎片的可使用的总激发能受到能量守恒规则的约束：
$$
TXE=Q_f-TKE
$$
因此，只要知道总动能（TKE）和原子核的质量，那么总激发能（TXE）就能确定。

**XE的分配**

### Hauser-Feshbach Statistical Decay

Hauser-Feshbach theory描述了复合核在统计平衡状态下逐渐衰变的过程，这一过程伴随着粒子和光子的逸出，最终达到基态或寿命较长的同位素状态。如图：

<img src="https://cgmf.readthedocs.io/en/latest/_images/decay_diagram.png" alt="_images/decay_diagram.png" style="zoom:67%;" />









