# GINSOLV 和 GINSPLOT

简体中文 | [English](README.md)

GINSOLV 是面向多星座、多频 GNSS 及 GNSS/INS 数据的后处理软件。用户可通过 Qt 界面选择处理配置、检查所需文件、运行解算并查看处理进度。

GINSPLOT 是配套的解算结果可视化与误差分析软件。

## 主要功能

- **SPP**：利用广播星历进行单点定位（Single Point Positioning）。
- **DGNSS**：利用流动站与基准站观测进行伪距差分定位（Differential GNSS）。
- **RTK**：基于载波相位的相对定位（Real-Time Kinematic），包括动态、静态、静态启动、移动基线及已知位置模式。
- **PPP**：精密单点定位（Precise Point Positioning），支持无电离层组合（Ionosphere-Free，IF）与非组合（Uncombined，UC）处理，使用精密轨道、钟差、天线和偏差产品。
- **PPP-AR**：模糊度固定的精密单点定位（Precise Point Positioning with Ambiguity Resolution），使用非组合（Uncombined，UC）观测以及配套的码和相位偏差产品。
- **PPP-RTK**：精密单点实时动态定位（Precise Point Positioning - Real-Time Kinematic），使用按物理频段组织的非组合（Uncombined，UC）观测、大气状态和区域改正。随包提供的筑波配置使用已录制的 QZSS CLAS L6 改正数据。
- **GNSS/INS 组合**：支持松耦合（Loosely Coupled，LC）与紧耦合（Tightly Coupled，TC）处理，可配置 IMU 特性、杆臂、安装角及辅助选项。

其他处理能力包括：

- GPS、GLONASS、Galileo、QZSS、BDS、NavIC 和 SBAS 观测；
- 显式选择物理频段的多频处理；
- 前向、后向、双向合并及不重置状态的双向合并处理；
- 可配置广播、精密、SBAS 和 SSR 星历来源；
- 电离层、对流层、天线、潮汐及地球自转改正；
- IGS、IGS-RTS、MADOCA、HAS 和 CLAS 改正服务输入；
- 可配置的模糊度固定、观测赋权、残差剔除及解算结果检验。

具体星座、信号、改正模型、模糊度处理方式及 GNSS/INS 组合的可用性，取决于处理配置以及提供的观测、产品和 IMU 数据。

## 输入

根据所选处理方法，GINSOLV 可使用：

- RINEX 格式的流动站与基准站 GNSS 观测；
- 已整理的手机 GNSS 观测和 IMU 测量数据；
- 广播导航电文；
- 精密轨道和钟差产品；
- 码和相位偏差产品；
- 天线校准、地球自转、电离层和海潮负荷产品；
- 已录制的 SSR、MADOCA、HAS 或 CLAS 改正消息；
- 接收机坐标和参考轨迹。

## 输出

GINSOLV 在配置指定的路径写入 `.pos` 解文件。根据启用的输出选项，文件可包含位置、速度、姿态、协方差及解质量字段。

可选的诊断输出包括：

- `.stat`：按配置的状态输出级别记录解状态、卫星状态和残差；
- `.trace`：按配置的跟踪级别记录处理诊断信息。

未启用的 STAT 或 trace 文件不会创建。

## MATLAB 安装与运行依赖

数值解算和绘图代码需要完整的 64 位 MATLAB 及有效的 MATLAB 许可证。Windows 程序使用 Windows MATLAB，Linux 程序使用原生 Linux MATLAB。仅安装 MATLAB Runtime 无法运行本软件的解算与绘图功能。

设置 `MATLAB_EXE` 后，GINSOLV 和 GINSPLOT 使用该变量指定的可执行程序。未设置时，程序搜索 PATH 和标准安装目录，并在 Windows 上查询 MATLAB 安装注册信息，默认选择已安装的最高版本。可将 `MATLAB_EXE` 指向 Windows 的 `bin/matlab.exe` 或 Linux 的 `bin/matlab` 来指定版本。

GINSPLOT 的 Satellite 视图在获取尚未缓存的卫星地图瓦片时需要联网。无法加载时，界面会提示网络错误，其他视图仍可使用。标准绘图及使用本地输入产品的解算可离线运行。

## 快速开始

1. 进入 `GINSOLV/bin`，在 Windows 上启动 `GINSOLV.exe`，或在 Linux 上运行 `./GINSOLV`。
2. 选择一份完整的 `prcopt_*.m` 配置文件。
3. 检查解析后的输入和输出路径。
4. 点击 **RUN**。

配置决定定位方法、星座、频段、模型、改正产品、处理方向、INS 耦合方式及输出文件。配置选项直接使用数值，每个选项旁的注释说明可选数值及其含义。

## GINSPLOT

GINSPLOT 读取 `.pos` 解文件、存在时关联的 `.stat` 文件，以及可选的 `reference.csv`。原生 C 代码负责文件读取和数据准备，Qt 提供用户界面，已安装的 MATLAB 负责绘图区。

可用视图包括：

- 标准轨迹图或卫星地图轨迹；
- 东、北、天（East、North、Up）速度分量；
- 俯仰、横滚、偏航（Pitch、Roll、Yaw）姿态分量；
- 带均方根误差（RMS）的定位、速度及姿态误差；
- 卫星数、模糊度比值及离散解状态；
- 伪距或载波相位残差时间序列及经验概率密度直方图；
- 伪距或载波相位残差与高度角、SNR 的关系；
- 按物理频段显示的 SNR。

System、Satellite、Band 和 Status 筛选支持多选。Residual 选项用于切换伪距（Code）和载波相位（Phase）残差。

查看解算结果：

1. 在 Windows 上启动 `GINSPLOT.exe`，或在 Linux 上运行 `./GINSPLOT`。
2. 选择 `.pos` 解文件。程序会关联同主文件名的配套文件，并搜索可用的参考文件。
3. 点击 **RUN**，随后在结果窗口选择视图和筛选条件。

## 发布目录

```text
release/                         (Windows)
|-- README.md
|-- README.zh-CN.md
|-- GINSOLV/
|   `-- bin/
|       |-- GINSOLV.exe
|       |-- GINSPLOT.exe
|       `-- core/
|           `-- ginsplotio.exe
`-- Data/

release-linux/                   (Linux)
|-- README.md
|-- README.zh-CN.md
|-- GINSOLV/
|   `-- bin/
|       |-- GINSOLV
|       |-- GINSPLOT
|       `-- core/
|           `-- ginsplotio
`-- Data/
```

随包提供的 `Data` 与 `GINSOLV` 位于同一级目录，包含输入数据、完整处理配置及参考轨迹。结果文件将在各配置指定的路径创建。

## 已测试环境

实际测试使用的环境为：

- 64 位 Windows，系统版本 26200.9457，MATLAB R2025b Update 1；
- WSL2 中的 Ubuntu 24.04.3 x86-64，原生 Linux MATLAB R2024b Update 3。
