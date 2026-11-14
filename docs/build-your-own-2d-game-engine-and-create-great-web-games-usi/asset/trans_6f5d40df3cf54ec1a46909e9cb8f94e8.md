
## 冲量的切向分量

现在，忽略角速度的切向分量，并在等式 (21) 两边同时对 `\hat{\mathrm{N}}` 向量执行点积，以分离法向分量：

```math
{\overrightarrow{\mathrm{V}}}_{AP2}\bullet \hat{\mathrm{N}}=\left\{ \left( {\overrightarrow{\mathrm{V}}}_{\mathrm{A}1}+ \frac{{\mathrm{j}}_{\mathrm{N}}}{{\mathrm{m}}_{\mathrm{A}}}\hat{\mathrm{N}}+ \frac{{\mathrm{j}}_{\mathrm{T}}}{{\mathrm{m}}_{\mathrm{A}}}\hat{\mathrm{T}} \right)+ \left( {\overrightarrow{\upomega}}_{\mathrm{A}1}+ \frac{{\mathrm{j}}_{\mathrm{N}}}{{\mathrm{I}}_{\mathrm{A}}} \left( {\overrightarrow{\mathrm{R}}}_{AP}\times \hat{\mathrm{N}} \right) \right)\times {\overrightarrow{\mathrm{R}}}_{AP} \right\}\bullet \hat{\mathrm{N}}
```

对右侧执行点积运算，认识到 `\hat{\mathrm{N}}` 是单位向量且垂直于 `\hat{\mathrm{T}}`，并令 `\overrightarrow{\mathrm{D}}={\overrightarrow{\mathrm{R}}}_{AP}\times \hat{\mathrm{N}}`；则该等式可重写如下：

```math
{\overrightarrow{\mathrm{V}}}_{AP2}\bullet \hat{\mathrm{N}}= \left( {\overrightarrow{\mathrm{V}}}_{\mathrm{A}1}\bullet \hat{\mathrm{N}} \right)+ \frac{{\mathrm{j}}_{\mathrm{N}}}{{\mathrm{m}}_{\mathrm{A}}}+ \left( {\overrightarrow{\upomega}}_{\mathrm{A}1}\times {\overrightarrow{\mathrm{R}}}_{AP} \right)\bullet \hat{\mathrm{N}}+ \frac{{\mathrm{j}}_{\mathrm{N}}}{{\mathrm{I}}_{\mathrm{A}}} \left( \overrightarrow{\mathrm{D}}\times {\overrightarrow{\mathrm{R}}}_{AP} \right)\bullet \hat{\mathrm{N}} \tag{23}
```

通过应用混合积恒等式并记住 `\overrightarrow{\mathrm{D}}={\overrightarrow{\mathrm{R}}}_{AP}\times \hat{\mathrm{N}}`，等式 (23) 中最右侧项的向量运算可简化如下：

```math
\left( \overrightarrow{\mathrm{D}}\times {\overrightarrow{\mathrm{R}}}_{AP} \right)\bullet \hat{\mathrm{N}}= \overrightarrow{\mathrm{D}}\bullet \left( {\overrightarrow{\mathrm{R}}}_{AP}\times \hat{\mathrm{N}} \right)= \left( {\overrightarrow{\mathrm{R}}}_{AP}\times \hat{\mathrm{N}} \right)\bullet \left( {\overrightarrow{\mathrm{R}}}_{AP}\times \hat{\mathrm{N}} \right) = {\left\| {\overrightarrow{\mathrm{R}}}_{AP}\times \hat{\mathrm{N}} \right\|}^2
```

通过这一处理并合并含点积的项，等式 (23) 变为以下形式：

```math
{\overrightarrow{\mathrm{V}}}_{AP2}\bullet \hat{\mathrm{N}}= \left( {\overrightarrow{\mathrm{V}}}_{\mathrm{A}1}+ \left( {\overrightarrow{\upomega}}_{\mathrm{A}1}\times {\overrightarrow{\mathrm{R}}}_{AP} \right) \right)\bullet \hat{\mathrm{N}}+ \frac{{\mathrm{j}}_{\mathrm{N}}}{{\mathrm{m}}_{\mathrm{A}}}+ \frac{{\mathrm{j}}_{\mathrm{N}}}{{\mathrm{I}}_{\mathrm{A}}} {\left\| {\overrightarrow{\mathrm{R}}}_{AP}\times \hat{\mathrm{N}} \right\|}^2
```

根据等式 (9)，右侧含点积的项简为 `\overrightarrow{\mathrm{V}}_{AP1}`：

```math
{\overrightarrow{\mathrm{V}}}_{AP2}\bullet \hat{\mathrm{N}}= {\overrightarrow{\mathrm{V}}}_{AP1}\bullet \hat{\mathrm{N}}+ \frac{{\mathrm{j}}_{\mathrm{N}}}{{\mathrm{m}}_{\mathrm{A}}}+ \frac{{\mathrm{j}}_{\mathrm{N}}}{{\mathrm{I}}_{\mathrm{A}}} {\left\| {\overrightarrow{\mathrm{R}}}_{AP}\times \hat{\mathrm{N}} \right\|}^2 \tag{24}
```

等式 (22) 可通过相同的代数运算步骤处理：忽略角速度的切向分量，并在等式两边同时对 `\hat{\mathrm{N}}` 向量执行点积；可推导出以下结果：

```math
{\overrightarrow{\mathrm{V}}}_{BP2}\bullet \hat{\mathrm{N}}= {\overrightarrow{\mathrm{V}}}_{BP1}\bullet \hat{\mathrm{N}}- \frac{{\mathrm{j}}_{\mathrm{N}}}{{\mathrm{m}}_{\mathrm{B}}}- \frac{{\mathrm{j}}_{\mathrm{N}}}{{\mathrm{I}}_{\mathrm{B}}} {\left\| {\overrightarrow{\mathrm{R}}}_{BP}\times \hat{\mathrm{N}} \right\|}^2 \tag{25}
```

用等式 (24) 减去等式 (25) 得到以下结果：

```math
\left( {\overrightarrow{\mathrm{V}}}_{AP2}-{\overrightarrow{\mathrm{V}}}_{BP2} \right)\bullet \hat{\mathrm{N}}= \left( {\overrightarrow{\mathrm{V}}}_{AP1}-{\overrightarrow{\mathrm{V}}}_{BP1} \right)\bullet \hat{\mathrm{N}}+ {\mathrm{j}}_{\mathrm{N}} \left( \frac{1}{{\mathrm{m}}_{\mathrm{A}}}+ \frac{1}{{\mathrm{m}}_{\mathrm{b}}}+ \frac{{\left\| {\overrightarrow{\mathrm{R}}}_{AP}\times \hat{\mathrm{N}} \right\|}^2}{{\mathrm{I}}_{\mathrm{A}}}+ \frac{{\left\| {\overrightarrow{\mathrm{R}}}_{BP}\times \hat{\mathrm{N}} \right\|}^2}{{\mathrm{I}}_{\mathrm{B}}} \right)
```

在左侧代入等式 (16) 后继之以等式 (13)，在右侧代入等式 (15)，得到以下结果：

```math
-\mathrm{e} \left( {\overrightarrow{\mathrm{V}}}_{ABP1}\bullet \hat{\mathrm{N}} \right)= {\overrightarrow{\mathrm{V}}}_{ABP1}\bullet \hat{\mathrm{N}}+ {\mathrm{j}}_{\mathrm{N}} \left( \frac{1}{{\mathrm{m}}_{\mathrm{A}}}+ \frac{1}{{\mathrm{m}}_{\mathrm{b}}}+ \frac{{\left\| {\overrightarrow{\mathrm{R}}}_{AP}\times \hat{\mathrm{N}} \right\|}^2}{{\mathrm{I}}_{\mathrm{A}}}+ \frac{{\left\| {\overrightarrow{\mathrm{R}}}_{BP}\times \hat{\mathrm{N}} \right\|}^2}{{\mathrm{I}}_{\mathrm{B}}} \right)
```

最后，合并项并求解 `j_N`：

```math
{\mathrm{j}}_{\mathrm{N}}= \frac{-\left( 1+\mathrm{e} \right) \left( {\overrightarrow{\mathrm{V}}}_{ABP1}\bullet \hat{\mathrm{N}} \right)}{ \frac{1}{{\mathrm{m}}_{\mathrm{A}}}+ \frac{1}{{\mathrm{m}}_{\mathrm{B}}}+ \frac{{\left\| {\overrightarrow{\mathrm{R}}}_{AP}\times \hat{\mathrm{N}} \right\|}^2}{{\mathrm{I}}_{\mathrm{A}}}+ \frac{{\left\| {\overrightarrow{\mathrm{R}}}_{BP}\times \hat{\mathrm{N}} \right\|}^2}{{\mathrm{I}}_{\mathrm{B}}} } \tag{26}
```

冲量的切向分量 `j_T` 可通过假设角速度法向分量的贡献极小并可忽略，以及通过在等式 (21) 和 (22) 两边同时对 `\hat{\mathrm{T}}` 向量执行点积来分离切向分量来进行近似：
