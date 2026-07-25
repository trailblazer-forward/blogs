---
title: katex-for-electronic-configuration
tags: technology
katex: true
date: 2026-07-25 17:47
---


# KaTeX 与电子排布

## 电子排布式

KaTeX 可以表示电子排布式是显然的。

例：Fe $\mathrm{1s^2 2s^2 2p^6 3s^2 3p^6 3d^6 4s^2}$

```tex
\mathrm{1s^2 2s^2 2p^6 3s^2 3p^6 3d^6 4s^2}
```

## 轨道表示式

然而，KaTeX 还能够渲染轨道表示式。

例：Al $\overset{\mathrm{1s}}{\begin{array}{|c|} \hline \uparrow\downarrow \\ \hline \end{array}} \quad
\overset{\mathrm{2s}}{\begin{array}{|c|} \hline \uparrow\downarrow \\ \hline \end{array}} \quad
\overset{\mathrm{2p}}{\begin{array}{|c|c|c|} \hline \uparrow\downarrow &\uparrow\downarrow & \uparrow\downarrow \\ \hline \end{array}} \quad
\overset{\mathrm{3s}}{\begin{array}{|c|} \hline \uparrow \\ \hline \end{array}} \quad
\overset{\mathrm{3p}}{\begin{array}{|c|c|c|} \hline \uparrow & \hspace{0.5em} & \hspace{0.5em} \\ \hline \end{array}}$

```tex
\overset{\mathrm{1s}}{\begin{array}{|c|} \hline \uparrow\downarrow \\ \hline \end{array}} \quad
\overset{\mathrm{2s}}{\begin{array}{|c|} \hline \uparrow\downarrow \\ \hline \end{array}} \quad
\overset{\mathrm{2p}}{\begin{array}{|c|c|c|} \hline \uparrow\downarrow &\uparrow\downarrow & \uparrow\downarrow \\ \hline \end{array}} \quad
\overset{\mathrm{3s}}{\begin{array}{|c|} \hline \uparrow \\ \hline \end{array}} \quad
\overset{\mathrm{3p}}{\begin{array}{|c|c|c|} \hline \uparrow & \hspace{0.5em} & \hspace{0.5em} \\ \hline \end{array}}
```

大体思路是这样的：

- 用 `\overset` 表示轨道
- 使用 `\begin{array}{|c|}` 创建元素两侧带分隔线的 `array` 环境
- 在开头用 `\hline`，结尾使用 `\\ \hline`，分别在上面和下面补上方框的上下边。
- 格子内部用 `\uparrow` 和 `\downarrow` 表示电子的自旋状态
- 对于空格子，里面塞一个 `\hspace{0.5em}` 防止格子太窄

每次都这么写未免也太冗长了，可以用宏替代。

```tex
\def\electronTrajectory#1#2#3{\overset{\mathrm{#1}}{\begin{array}{#2} \hline #3 \\ \hline \end{array}}}
\def\etS#1#2{\electronTrajectory{#1s}{|c|}{#2}}
\def\etP#1#2{\electronTrajectory{#1p}{|c|c|c|}{#2}}
\def\etD#1#2{\electronTrajectory{#1d}{|c|c|c|c|c|}{#2}}
\def\etF#1#2{\electronTrajectory{#1f}{|c|c|c|c|c|c|c|}{#2}}
\def\emptyT{\hspace{0.5em}}
```

然后就直接可以使用 `\etS{1}{\uparrow}` 来渲染出来 $\mathrm{1s}$ 轨道的上箭头, `\etP{2s}{\uparrow \downarrow}` 渲染 $\mathrm{2s}$ 的上箭头和下箭头了。

这个思路挺离奇的。
