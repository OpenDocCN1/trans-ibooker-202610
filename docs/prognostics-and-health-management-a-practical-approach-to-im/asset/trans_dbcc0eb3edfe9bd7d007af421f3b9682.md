

## Feature Vector Framework

Feature Vector 块包含三个部分——主部分、数据平滑部分和 FFP 到 DPS 转换部分：

FD = feature data = FD0*(1 + dP/P0)
FFP = (FD – FD0 – NM)/FD0 where NM = FNM*FD0/100 or
FFP = (FD – FD0 – NM) when FDNOMDIV = 0

主部分包含六个条目用于指定将 CBD 转换为 FD 的变换：

- *FNM* 指定 NM，即标称 FD 值（FD0）的百分比
- *FAILM* 指定百分比失效裕度：70 表示当 FFP = 0.70 时失效
- *FDNOM* 指定被视为标称 FD 值（FD0）的数据值
- *FDNOMV* 指定 FD0 的预期百分比方差
- *FDNOMMIN* 指定用于 FD0 的最小值。
  - 当测量的 FD0 小于 FDNOMMIN 时，使用 FDNOMIN。
  - 否则使用测量的 FD0。
- *FDNOMDIV* 指定是否执行除法（非零值）或不执行（零值）。

数据平滑部分包含四个条目用于数据平滑：

- *FD0PTS* 指定用于平均的 CBD 点数，以计算 FD0 的值。
  - 当指定为零（0）时，使用 FDNOM。
  - 当计算中的方差超过 FDNOMV 时，使用 FDNOM。
- *FDPTS* 指定用于平滑的 FD 点数进行平均。
- *FFPPTS* 指定用于平滑的 FFP 点数进行平均。
- *FFSPTS* 指定用于平滑的 FFS 点数进行平均。

FFP 到 DPS 转换部分包含两个条目用于将 FFP 特征数据转换为 DPS 数据：

- *XDPS* 指定用于选择 FFP 到 DPS 转换模型的 FFP 模型：
  - 0 表示不转换。
  - 1 表示使用 FFP = FD0*(dP/P0)^n。
  - 2 表示使用 FFP = FD0*([1/(1 - dP/P0)]^n - 1)。
  - 3 表示使用 FFP = FD0*(1-[1/(1 + dP/P0)]^n)。
  - 4 表示使用 FFP = FD0*((1 + dP/P0)^n - 1)。
  - 5 表示使用 FFP = FD0*(1 - (1 - dP/P0)^n)。
  - 6 表示使用 FFP = FD0*(exp(dP/P0) - 1)。
  - 7 表示使用 FFP = FD0*(1 - exp(-dP/P0))。
- *XDPSNV* 指定用于 n 或 lambda（寿命）的值。

## Prediction Framework

预测框架部分（图 6.61）包含两个条目用于指定在生成预测信息时使用的值：

- *PITTFF0* 指定退化开始后失效时间（TTF）值：
  - 默认值为 200 个采样周期。
- *PIFFSMOD* 指定输入数据的预期形状：
  - 1 表示凹形特征曲线。

```
%**Feature Vector          (A)
FNM      = 2.5 ;      % Noise margin - % of FD0
FAILM    = 70 ;       % Failure margin - % above 0
FDNOM    = 0 ;        % Feature Data (FD), Nominal value
FDNOMV   = 3 ;        % FD Nominal variance (%)
FDNOMMIN = 1 ;        % minimum nominal value: if less, perform DIV
FDNOMDIV = 0 ;        % 0 = subtract only (no division) - small signal
                      % else FFP = (FD - DIV)/DIV
```

```
(B)
FD0PTS   = 20 ;       % # data points to average to calculate FD0
FDPTS    = 4 ;        % Rolling FD (CBD) points to average: up to 4
FFPPTS   = 4 ;        % Rolling FFP points to average: up to 4
FFSPTS   = 4 ;        % Rolling FFS points to average: up to 4
```

```
%**FFP to DPS Transform    (C)
XDPS     = 0 ;        % none
%        = 1 ;        % FFP = FD0*(dP/P0)^n
%        = 2 ;        % FFP = FD0*([1/(1 - dP/P0)]^n - 1)
%        = 3 ;        % FFP = FD0*[1-{1/(1 + dP/P0)}^n]
%        = 4 ;        % FFP = FD0*([1 + dP/P0]^n - 1)
%        = 5 ;        % FFP = FD0*(1 - (1 - dP/P0)^n)
%        = 6 ;        % FFP = FD0*(exp(dP/P0) - 1)
%        = 7 ;        % FFP = FD0*(1 - exp(-dP/P0))
XDPSNV   = 1 ;        % n or Lambda (life) value
```

Figure 6.60 NDEF: feature-vector framework – (a) primary, (b) smoothing, (c) FFP-DPS transform.

```
%**Prediction (Prognostic) Information
PITTFF0  = 4800 ;     % estimated time (hours) after onset of damage
PIFFSMOD = 2 ;       % model 1=Concave, 2=Linear, 3=Convex, 4=complex
PIFFSNM  = 3 ;       % noise margin (%) for estimating begin degradation
```

Figure 6.61 NDEF: Prediction Framework.

- 2 表示线性曲线——这是默认值。
- 3 表示凸形曲线。
- 4 表示复杂（不规则）曲线。

- *PIFFSNM* 指定用于估计退化开始（BD）的 NM（%）。

关于 PIFFSMOD 的操作说明：您的 PHM 系统预装有一套基本的预测算法嵌入在名为 DXARULE（自适应剩余使用寿命估计器）的软件程序中，无需额外付费，因为它不提供对非线性输入数据的特殊处理（PIFFSMOD 被忽略）。您的客户可以选择获取并使用高级版本 ARULEAV。

## Performance Services/Graphics

性能服务/图形部分（图 6.62）包含三个与性能相关的规范条目：

- *PGSW* 为未来计划的功能（图形用户界面）保留。
- *PGALPHA* 指定用于评估预测精度的 alpha（精度）百分比。
- *PSSW* 指定是否（1）或不（0）将性能指标写入日志文件。

```
%**Performance Services/Graphics
PGSW      = 1 ;        % Future
PGALPHA   = 10 ;       % Prognostic alpha for Prognostic Horizon
PSSW      = 1 ;        % Write PI results to file, 0 = no, 1 = yes
```

Figure 6.62 NDEF: Performance Services/Graphics.

```
%**Input & Output Files***********************************
FDINLIB   = 'C:\DXPHMLIB\DXINDATA\' ;  % Input data library
OUTLIB    = 'C:\DXPHMLIB\DXOUTDATA\' ; % Output data library
INFILE    = 'DXDATFR1 ' ;               % File name for input data
OUTFILE   = 'DXDATFR1 ' ;               % File name for output data
INTYPE    = '.txt' ;                    % Input file type, text
OUTTYPE   = '.csv' ;                    % Output file type, text
```

Figure 6.63 NDEF: Input & Output Files.

## Input & Output Files

输入和输出文件部分（图 6.63）包含六个条目用于指定数据的输入和输出文件：

- *FDINLIB* 指定输入数据库。
- *OUTLIB* 指定输出数据库。
- *INFILE* 指定输入数据文件的名称。
- *OUTFILE* 指定输出数据文件的名称。
- *INTYPE* 指定输入数据文件的类型（.txt 为默认值）。
- *OUTTYPE* 指定输出数据文件的类型（.csv 为默认值）。

## Checkpoint Library and File Names

工作文件部分（图 6.64）包含九个条目用于指定保存和恢复操作环境以支持检查点和重启的工作文件的库名称、名称前缀和类型：

- *CPTLIB* 指定用于工作文件的检查点（和恢复）库。
- *CPTTYP* 指定工作文件的类型。
- *DDWFN* 指定设备驱动程序的工作文件名称前缀。
- *FVWFN* 指定特征向量程序的工作文件名称前缀。

```
%**Checkpoint Library & File Name**************************
CPTLIB    = 'C:\DXPHMLIB\DXCPT\' ;      % Checkpoint restore
CPTTYP    = 'csv'                     % file name type
DDWFN     = 'DDW_' ; % Device Driver work
FVWFN     = 'FVW_' ; % Feature Vector work
PIWFN     = 'PIW_' ; % Prognostic Information work
PSWFN     = 'PSW_' ; % Prognostic Services work
VSWFN     = 'VSW_' ; % Visual Services work (future)
EFWFN     = 'EFW_' ; % End File Services work (future)
PAWFN     = 'PAW_' ; % Performance Analysis work
```

Figure 6.64 NDEF: Checkpoint Library & File Name.

- *PIWFN* 指定用于预测信息程序的工作文件名称前缀。
- *PSWFN* 指定用于预测服务程序的工作文件名称前缀。
- *VSWFN* 指定用于可视化服务程序的工作文件名称前缀。
- *EFWFN* 指定用于结束文件程序的工作文件名称前缀。
- *PAWFN* 指定用于性能分析程序的工作文件名称前缀。

## Device Driver Program ID and Data Units of Measure

数据单位测量部分（图 6.65）包含三个条目用于定义输入数据的测量单位：

- *DDPID* 指定设备驱动程序的程序标识符。
- *DDUT* 指定用于时间单位的文本。
- *DDUA* 指定用于幅度单位的文本。

您选择此格式的原因是，未来您可能会决定为其他形式的数据指定测量单位，例如来自特征向量程序（FV PID）的数据。

## Program Identifiers

程序标识符部分（图 6.66）包含六个条目用于指定除设备驱动程序之外用于处理节点的程序的标识符（数值）：

- *FV PID* 指定特征向量程序的数字标识符。
- *PIPID* 指定预测信息程序的数字标识符。
- *PSPID* 指定预测服务程序的数字标识符。
- *VSPID* 指定可视化服务程序的数字标识符。
- *EFPID* 指定结束文件程序的数字标识符。
- *PAPID* 指定性能分析程序的数字标识符。

## End of Definition

程序标识符部分后面跟着一个结束定义（ENDDEF）条目（图 6.67），作为明确的结束而不是隐含的结束。

```
%**Device Driver Program ID and Units of Measure**************
DDPID    = 1 ;    % Required entry    DXDDTXT01
DDUT     = '[hr]' ; % Units of time
DDUA     = '[kHz]' ;% Units of amplitude
```

Figure 6.65 NDEF: Device Driver Program ID and Units of Measure.

```
%**Other Program IDs******************************************
FV PID   = 1 ;    % Required entry    DXFV1S01
PIPID    = 2 ;    % Required entry    DXARULE
PSPID    = 1 ;    % Optional entry    DXPS01
VSPID    = 0 ;    % Place holder
EFPID    = 0 ;    % Place holder
PAPID    = 0 ;    % Optional entry    DXPA01
```

Figure 6.66 NDEF: Other Program IDs.

