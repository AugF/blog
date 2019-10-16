---
title: tools-latex-MathJax
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-10-14 10:34:04
tags:
- tools
- latex
categories:
- [tools, latex]
---

https://colobu.com/2014/08/17/MathJax-quick-reference/

MathJax是一个JavaScript引擎，用来显示网络上的数学公式。它支持大部分主流的浏览器，对大部分用户而言它不需要安装，既没有插件需要下载也没有软件需要安装，属于Apache。MathJax使用网络字体去产生高质量的排版，使其在所有分辨率都可缩放和显示，这远比包含公式的图片更有效得多。使用MathJax显示数学公式是基于文本的，而非图片。他可以被搜索引擎使用，这意味着方程式和页面上的文字一样是可以被搜索的。MathJax允许页面作者使用Tex、 LaTex符号和MathML或者AsciiMath去书写公式。 转化为MathML格式你可以赋值粘贴它们到其他程序中。
MathJax是模块化的，所以它仅仅在需要时才加载它的组件，同时也可以被扩展以实现更多功能。MathJax同时也是高度可配置的，允许作者做出更适宜网站自身的定义。

[MathJax中文文档](https://colobu.com/2014/08/17/MathJax-quick-reference/)


1. 希腊字母 
    - lower
    `\alpha, \beta, \gamma, \delta, \epsilon(\varepsilon), \zeta, \eta, \theta(\vartheta), \iota, \kappa, \lambda, \mu, \xi, o, \pi, \rho(\varrho), \sigma, \tau, \upsilon, \phi(\varphi), \chi, \psi, \omega`
    $$\alpha, \beta, \gamma, \delta, \epsilon(\varepsilon), \zeta, \eta, \theta(\vartheta), \iota, \kappa, \lambda, \mu, \xi, o, \pi, \rho(\varrho), \sigma, \tau, \upsilon, \phi(\varphi), \chi, \psi, \omega$$
    - upper
    `A, B, \Gamma, \Delta, E, Z, H, \Theta, I, K, \Lambda, M(N), \Xi, O, \Pi, P, \Sigma, T, \Upsilon, \Phi, X, \Psi, \Omega`
    $$A, B, \Gamma, \Delata, E, Z, H, \Theta, I, K, \Lambda, M(N), \Xi, O, \Pi, P, \Sigma, T, \Upsilon, \Phi, X, \Psi, \Omega$$
2. 字体
    - `\mathbb, \Bbb`黑板体  $\mathbb{Z}, \Bbb{Z}$
    - `\mathbf`粗体  $\mathbf{Z}$
    - `\mathtt`打印体 $\mathtt{Z}$
    - `\mathrm`罗马体 $\mathrm{Z}$
    - `\mathcal` $\mathcal{Z}$
    - `\mathscr` $\mathscr{Z}$
    - `\mathfrak` $\mathfrak{Z}$

3. 一大堆符号
    > https://pic.plover.com/MISC/symbols.pdf
    - 常用

4. 中途空格 `\quad, \qquad`
5. `\hat, \widehat, \bar, \overline, \vec, \overrightarrow, \dot, \ddot`
    > $\hat{x}, \widehat{xy}, \bar{x}, \overline{xyz}, \vec{x}, \overrightarrow{xyz}, \dot{x}, \ddot{x}$

6. 矩阵 `\begin{matrix} ... \end{matrix}`
        