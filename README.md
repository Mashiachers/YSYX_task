# 一生一芯·F3 数字逻辑电路基础设计实现 (OSCPU Digital Circuits)

本项目为在**中国科学院计算技术研究所 / 南京大学**等联合发起的「**一生一芯（One Student One Chip）**」预学习准备阶段（F阶段）中，基于 **Logisim-evolution 4.1.0** 自主设计、仿真、调试并形式化验证的全部数字逻辑电路工程库。

---

## 📚 长期维护的工程设计与调试全纪录
> **👉 [点击查阅：F3 全程设计与调试实战演进日志 (F3_CIRCUIT_DESIGN_AND_DEBUG_LOG.md)](./F3_CIRCUIT_DESIGN_AND_DEBUG_LOG.md)**  
> 记录每一次电路迭代的**设计动机、踩坑现象、假设验证、底层物理成因（Root Cause）、架构演进与体系结构哲学**（从 CMOS 物理门、优先编码器三次攻坚、原码加法器 SMA1/SMA2 重构，到反码与补码，长期维护更新至 F3 完全结束）。

---

## 📁 仓库文件结构

* **`or_gemini.circ`**：最新全量工程电路源文件（包含当前主电路 `OnesComplementAdder` 及全部 28 个子电路）；
* **`digital_circuits.circ`**：稳定版全量工程电路归档；
* **`F3_CIRCUIT_DESIGN_AND_DEBUG_LOG.md`**：**【核心官方日志】全程设计与调试实战演进日志（长期更新）**；
* **`digital_design_doc.md`**：系统级架构复盘与原理技术文档；
* **`README.md`**：项目概览与使用指南。

---

## 🛠️ 当前包含的电路清单 (共 28+ 个电路模块)

### 1. 物理晶体管与门电路层
* `or_gate`：CMOS 互补对称或门（上拉 PMOS + 下拉 NMOS + 反相器）
* `Xor_gate`：最少晶体管优化方案异或门
* `XNOR_gate` / `XNOR_gatequan`：全晶体管同或门

### 2. 编解码与显示控制
* `decoder_circuit`：2-4 译码器（最小项 $m_0 \sim m_3$ 捕获）
* `decoder_3to8`：基于高位使能端级联扩展的 3-8 译码器
* `seven_code_trans`：十进制七段数码管译码器 (0~9)
* `seven_code_16_trans`：十六进制七段数码管全状态驱动器 (0~F)
* `seven_code_16or10_trans`：数码管进制模式动态切换器
* `encoder`：4-2 普通编码器
* `encoder_privil`：4-2 优先编码器
* `encoder_16to4_true1`：16-4 优先编码器（**4 组独立 OR 门 + 行波互斥优先级使能屏蔽链**）
* `encoder_16to4_gemini`：优化版行波互斥优先级仲裁编码器

### 3. 数据路由与数值比较
* `mux1`：1位 2选1 多路选择器
* `MUX_3bit_4to1`：3位宽 4选1 多路选择器
* `Comparator_4bit`：4位数值判等比较器 ($A == B$)

### 4. 算术运算部件 (加法器与减法器)
* `halfadder_1bit`：1位半加器
* `fulladder_1bit`：1位全加器（标准积之和 SOP 两级与或门）
* `fulladder_1bit_ano`：1位全加器（3个半加器级联，进位保留与权值对齐实现）
* `substractor_1bit`：1位全减器（卡诺图推导与最小项最大化复用）
* `adder_4bit`：4位行波进位加法器 (RCA)，带十六进制显示与进位 LED
* `subtractor_4bit`：4位行波借位减法器 (RBS)，带十六进制显示与借位 LED

### 5. 原码加法系统微架构
* `SignMagnitudeAdder_4bit`：4位原码加法器原型（符号分离 + 3位绝对值比较器 + 双 MUX 操作数对齐 + 符号仲裁）
* `SMA_4bit_1`：4位原码加法器模块化封装 IP 核（规范化输入 A, B 与输出 Y0, Y1）
* `SMA_4bit_2`：深度优化版原码加法器（**手搓 6 输入 NOR 门全零判决**，双轨总线解耦，进位重构送入 $m_3$ 突破十六进制 $0 \sim E$ 满量程显示）

### 6. 反码运算与转换系统 (当前主电路)
* `SignMagnitude_To_OnesComplement_Converter`：**原码-反码双向原子转换核**（3 个异或门实现正数直通、负数数值按位取反）
* **`OnesComplementAdder` (★ main 主电路)**：4 位反码加法器系统（输入反码 $	o$ 转原码 $	o$ 复用 `SMA_4bit_1` 核心 $	o$ 转回反码）

---

## 💻 运行与仿真方式

1. 打开 [Logisim-evolution](https://github.com/logisim-evolution/logisim-evolution) (4.1.0 或以上版本)；
2. 选择 `File -> Open`，打开 `or_gemini.circ` 或 `digital_circuits.circ`；
3. 工程默认已将 **`OnesComplementAdder`** 设为主电路，双击即可进入查看顶层交互原理图；
4. 按快捷键 `Alt + 1` (Poke Tool) 拨动输入引脚的二进制值，观察七段数码管动态计算与显示。
