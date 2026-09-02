# 一生一芯·F3 数字逻辑电路基础设计实现 (OSCPU Digital Circuits)

本项目为在**中国科学院计算技术研究所 / 南京大学**等联合发起的「**一生一芯（One Student One Chip）**」预学习准备阶段（F阶段）中，基于 **Logisim-evolution 4.1.0** 自主设计、仿真并验证的全部数字逻辑电路工程库。

---

## 📁 仓库文件结构

* **`digital_circuits.circ`**：全量 26 个子电路完整工程文件（推荐在 Logisim 中直接打开此文件）；
* **`or_gemini.circ`**：同步全量工程电路文件；
* **`digital_design_doc.md`**：详细的**系统级设计复盘与架构技术文档**（包含各子模块的设计思路、踩坑记录、数学证明与微架构图）；
* **`README.md`**：项目概览与使用指南。

---

## 🛠️ 包含的电路清单 (共 26 个子电路)

### 1. 物理晶体管与门电路层
* `or_gate`：CMOS 互补对称或门
* `Xor_gate`：最少晶体管方案异或门
* `XNOR_gate` / `XNOR_gatequan`：全晶体管同或门

### 2. 编解码与显示控制
* `decoder_circuit`：2-4 译码器
* `decoder_3to8`：基于 2-4 级联扩展的 3-8 译码器
* `seven_code_trans`：十进制七段数码管译码器 (0~9)
* `seven_code_16_trans`：十六进制七段数码管全状态驱动器 (0~F)
* `encoder`：4-2 普通编码器
* `encoder_privil`：4-2 优先编码器
* `encoder_16to4_true1`：16-4 优先编码器（独立 OR 门与互斥优先级使能屏蔽链）
* `encoder_16to4_gemini`：优化版行波互斥优先级仲裁编码器

### 3. 数据路由与数值比较
* `mux1`：1位 2选1 多路选择器
* `MUX_3bit_4to1`：3位宽 4选1 多路选择器
* `seven_code_16or10_trans`：数码管进制模式切换器
* `Comparator_4bit`：4位数值比较器

### 4. 算术运算部件 (加法器与减法器)
* `halfadder_1bit`：1位半加器
* `fulladder_1bit`：1位全加器（标准积之和式 SOP 两级与或门，极限门延迟）
* `fulladder_1bit_ano`：1位全加器（3个半加器级联，进位保留权值对齐实现）
* `substractor_1bit`：1位全减器（最小项资源复用）
* `adder_4bit`：4位行波进位加法器 (RCA)，带输入/输出十六进制数码管显示与进位 LED
* `subtractor_4bit`：4位行波借位减法器 (RBS)，带输入/输出十六进制数码管显示与借位 LED

### 5. 系统级原码运算微架构
* `SignMagnitudeAdder_4bit`：4位原码加法器原型系统（符号分离 + 3位数值比较器 + 双 MUX 操作数动态对齐 + 符号仲裁 + 负号独立数码管指示）
* `SMA_4bit_1`：4位原码加法器模块化封装 IP 核（规范化标准输入 A, B 与输出 Y0, Y1）

---

## 💻 运行与仿真方式

1. 下载并安装 [Logisim-evolution](https://github.com/logisim-evolution/logisim-evolution) (推荐 4.1.0 或以上版本)；
2. 在 Logisim 中选择 `File -> Open`，打开 `digital_circuits.circ`；
3. 双击左侧 Explorer 中的任意子电路即可进入查看原理图；
4. 使用快捷键 `Alt + 1` (Poke Tool) 点击拨码开关 (DipSwitch) 或输入引脚进行动态交互仿真。
