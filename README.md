# 一生一芯·F3 数字逻辑电路基础设计实现 (OSCPU Digital Circuits)

本项目为在**中国科学院计算技术研究所 / 南京大学**等联合发起的「**一生一芯（One Student One Chip）**」预学习准备阶段（F阶段）中，基于 **Logisim-evolution 4.1.0** 自主设计、仿真、调试并形式化验证的全部数字逻辑电路工程库。

🎉 **里程碑达成：F3 阶段全部电路与时序微架构设计圆满收官（100% Completed）！**

---

## 📚 长期维护的工程设计与调试全纪录
> **👉 [点击查阅：F3 全程设计与调试实战演进日志 (F3_CIRCUIT_DESIGN_AND_DEBUG_LOG.md)](./F3_CIRCUIT_DESIGN_AND_DEBUG_LOG.md)**  
> 详细记录全阶段 10 个章节的**设计动机、踩坑现象、假设验证、底层物理成因（Root Cause）、架构演进与体系结构哲学**（从 CMOS 物理门、优先编码器三次攻坚、原码加法器 SMA1/SMA2 重构、反码与补码，到主从触发器、同步状态机、1~10 自然数累加器与 60 进制工业级数字钟）。

---

## 📁 仓库文件结构

* **`or_gemini.circ`**：最新全量工程电路源文件（包含全部 51 个子电路与顶层微架构）；
* **`digital_circuits.circ`**：稳定版全量工程电路归档；
* **`F3_CIRCUIT_DESIGN_AND_DEBUG_LOG.md`**：**【核心官方日志】全程设计与调试实战演进日志（长期维护，第 1~11 章全景完结）**；
* **`digital_design_doc.md`**：系统级架构复盘与原理技术文档；
* **`README.md`**：项目概览与全量电路使用指南。

---

## 🛠️ 当前包含的电路清单 (共 51 个电路模块)

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
* `encoder_16to4` / `encoder_16to4_true`：16-4 优先编码器架构探索版
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

### 6. 反码运算与转换系统
* `SignMagnitude_To_OnesComplement_Converter`：**原码-反码双向原子转换核**（3 个异或门实现正数直通、负数数值按位取反）
* `OnesComplementAdder`：4 位反码加法器系统（输入反码 $\to$ 转原码 $\to$ 复用 `SMA_4bit_1` 核心 $\to$ 转回反码）
* `OnesComplementAdder_1`：**纯门级循环进位反码加法器**（双级 RCA 级联，基于自适应补偿公式 $\text{AddOne} = (A_3 \cdot B_3) \lor ((A_3 \oplus B_3) \cdot \overline{S_3})$，100% 满分通过）
* `OnesComplementAdder_2`：**精简架构反码加法器**（基于硬件模数数学定理，将首级 RCA 的 $C_{out}$ 直接作为二级加法器末位进位 +1，架构精炼，200/200 满分通过）

### 7. 补码与溢出检测系统 (ALU 核心)
* `adder_4bit_overflow`：双加法器溢出探索电路（打补丁式设计演进归档）
* `adder_4bit_overflow_true`：**极简单门代数收敛溢出加法器**（双级加法器并行提取次高位进位 $C_3$，单个 XOR 门实现 $V = C_4 \oplus C_3$，512/512 100% 满分通过）

### 8. 时序逻辑与触发器体系
* `SR_Latch`：双 NOR 门交叉耦合基础 SR 锁存器
* `SR_Latch_1`：带使能控制端的门控 SR 锁存器（深入剖析亚稳态震荡微观成因）
* `D_Latch`：D 锁存器（强制 $S=D, R=\overline{D}$ 杜绝非法输入态）
* `DFF`：**主从边沿触发 D 触发器**（双级 D 锁存器反相时钟级联，彻底消除电平透明性）
* `DFF_1`：带同步/异步复位控制的 D 触发器
* `DFF_2`：带手动步进脉冲按键接口（Button）的 D 触发器
* `DFF_3`：**带自反馈使能保持（Load Enable）的工程化 D 触发器**（通过 MUX 实现 $EN=0$ 保持原值、$EN=1$ 锁存新值）
* `DFF_reverse`：**无振荡位翻转器**（利用主从 DFF 实现 $D = \overline{Q}$ 闭环，稳定 2 分频计数器原型）

### 9. 寄存器与多位同步计数器系统
* `Register_4bit`：4 位并行数据寄存器（4 组 `DFF_3` 阵列 + `seven_code_16_trans` 十六进制动态数码管显示驱动）
* `Counter_4bit`：4 位自闭环同步计数器系统（`Register_4bit` 现态输出 $\to$ `adder_4bit_overflow_true` 增量 $+1$ $\to$ 次态写回）
* `Register_8bit`：**8 位并行数据寄存器**（双 `Register_4bit` 级联，支持统一使能与双数码管直读 `0x00`~`0xFF`）
* `adder_8bit`：**8 位行波进位全加器**（双 `adder_4bit` 级联，进位链无缝贯通）
* `Counter_8bit`：**8 位工业级全同步计数器**（内置 4 选 1 MUX，完备实现保持、递增、清零状态转移，复位 CLR 具有最高优先级）

### 10. 系统级微架构与算法/时钟应用（F3 终极收官里程碑）
* **`Sequence_1to10`（自然数 1~10 累加求和系统）**：
  * 基于 `Counter_8bit`（步进序列生成器）、`adder_8bit`（累加核心）与 `Register_8bit`（累加状态寄存器）构成的闭环数据通路；
  * 全硬件自动执行 $1 + 2 + \dots + 10 = 55$（十六进制 `0x37`），完美验证状态机与数据通路的闭环协同。
* **`Clock_digital`（60进制工业级数字钟系统）**：
  * 秒/分级联多位数字钟，驱动 4 组数码管实时呈现时间；
  * **单全局 1 Hz 时钟树**驱动，基于“在稳态 59 处前瞻采样预判”的现代时序逻辑架构，彻底消除“60 自杀式毛刺回环”，实现零毛刺、零冒险的纯同步分级使能与清零。

### 11. 简单处理器 sCPU 微架构核心部件与整机 (F5 阶段)
* **`ROM_8MUL8`（指令存储器与取指通路，Instruction ROM & Fetch Unit）**：
  * 基于 8 选 1 的 8 位宽 MUX 实现，内部预存数列求和程序 8 条机器码（`0x8a`, `0x90`, `0xa0`, `0xb1`, `0x17`, `0x29`, `0xd1`, `0xdf`）；
  * 配合 3 位 PC 寄存器、加法器与 `datain` 选通 MUX 构成自增（$PC+1$）与分支跳转闭环取指通路。
* **`RAM_8MUL8`（通用寄存器堆，General Purpose Registers / RAM）**：
  * 采用工业级标准微架构：写数据总线（`wdata`）全并联广播 + 写地址（`waddr`）经 3-8 译码器进行写使能（`EN`）门控，彻底杜绝非选中单元被误抹为 0；
  * 8 单元 × 8 位宽独立数据寄存器阵列，8 选 1 读 MUX 实时直读（`rdata`），接口完全独立解耦。
* **`sCPU_full`（单周期通用简单处理器顶层，Single-Cycle sCPU Processor）**：
  * 完整实现三类 sISA 指令：立即数加载 `li`（`10`）、算术加法 `add`（`00`）、条件分支跳转 `bne`（`11`）；
  * **取指/译码/执行/写回全闭环**：包含双读端口寄存器堆（`rs1`, `rs2` 直读）、8 位 ALU 加法器、位扩展 AND-OR 结果选通网络；
  * **动态分支闭环**：8 位比较器（Comparator）检测 $rdata2 \neq r0$，命中时驱动 PC 载入跳转目标地址（`datain`），全自动跑通 $1 + 2 + \dots + 10 = 55$（十六进制 `0x37`）自然数累加求和程序；
  * **全局同步复位网络**：`CLR` 一键同步复位程序计数器 PC 与全量通用寄存器。

---

## 💻 运行与仿真方式

1. 打开 [Logisim-evolution](https://github.com/logisim-evolution/logisim-evolution) (4.1.0 或以上版本)；
2. 选择 `File -> Open`，打开 `or_gemini.circ` 或 `digital_circuits.circ`；
3. 双击左侧电路树中的任意电路（例如 `Clock_digital` 或 `Sequence_1to10`）进入交互原理图；
4. 按快捷键 `Ctrl + K` 开启自动时钟走拍，或按快捷键 `Alt + 1` (Poke Tool) 拨动输入引脚/按键观察电路行为。
