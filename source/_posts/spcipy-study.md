---
title: spcipy study
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-12 14:24:20
tags:
- machinelearning
- scipy
categories:
- [machinelearning, scipy]
---

## 概览

scipy库依赖于numpy库，它提供了便捷且快速的N维数组操作。构建scipy库的原因是，它能与numpy数组一起工作，并提供许多用户友好和高效的数字实践，例如数值积分和优化的教程
numerical integration, interpolation插值， optimization, linear algebra线性代数 statistics统计

- cluster： cluster algorithms
    - vq: 只支持矢量化 vector quantization, k-means algorithms
        - whiten: normalize a group of observations on a per feature basis
        - vq: assign codes from a code book to observations
        - kmeans: 
    - hierarchy: hierarchical and agglomerative clustering, 支持层次聚类和凝聚聚类; 1. generating hierarchical clusters from distance matrices 2. calculating statistics on clusters 3. cutting linkages to generate flat clusters 4. visualizing clusters with dendrograms
- constants: physcial and mathematical constants
    - math
        - pi
        - golden
        - golden_ratio
    - physical
        - c: 光速
    - quick getting:  constants.value(u"elementary charge"), unit, precision, find
- fftpack: fast fourier transform routines
    - fft: discrete 离散, both real or complex sequence
    - ifft: inverse相反
    - fft2, ifft2: 2-d
    - fftn, ifftn
    - dct 余弦变换
    - dst: discrete sine transform
- integrate: integration and orginary differential equation slovers
    - quad: definite integral: 有限积分，三个参数，函数、上下限
    - dblquad: dobule类型
    - tplquad: 计算多重积分的表达式, 积分限是函数
    - nquad: 多重积分，积分限是具体的值
    - fixed_quad: 计算一个固定的积分使用fixed-order的高斯分布
    - trapz: 使用复合梯度算法计算, 同理simpz, romb
- interpolate: 插值, interpolation and smoothing splines
    - interp1d
- io: input and output
    > loadmat, savemat,可以加载matlab文件
- linalg: linear algebra
    - basic
        - inv
        - solve(a,b)  a*x=b
        - solve_banded(): a是带状矩阵
        - solve_triangular(a,b): 解决三角举证
        - det 行列式
        - norm 范数
        - lstsq() 二乘解
        - pinv: 矩阵的伪逆
        - kron: 元素乘积
    - eigenvalue problems: 特征值问题
        - eig: 方阵特征值问题
    - Decompositions
        - lu: LU分解
        - svd： 奇异值分解
        - qr：QR分解
    - Matrix Functions
        - expm: 使用近似方法
    - Matrix Equation Solvers
- ndimage: n-dimensional image processing
    - convolve(): 多维卷积操作
    - gaussian_filter(): 高斯过滤
    - laplace： 拉普拉斯过滤
    - floutier_ellipsoid: 多维ellipsoid fourier filter
- odr: orthogoal distance regression 正交距离回归问题
- optimize: optimization and root-finding routines
    - Optimization
        - scalar function标量函数: minimize_scalar
        - Local(Multivariate) Optimization: 多元回归
        - Global Optimization: brute()
        - Least-squares and Curve Fitting
            - nonlinear
            - linear
            - curve fitting
        - Root finding
    - Linear Programming
        - linprog
    - Utilities
        - Finite-Difference Approximation
            - approx_fprime
            - check_grad
        - Line Search
        - Hessian Approximation
        - Benchmark Problems
    - Legacy Fuctions 遗留功能
        - fmin
- signal: signal processing
    - convolution
        - convolve
    - B-splines
        - bspline, cubic
    - Filtering
    - Filter design
    - Matlab-style IIR filter design: butter, cheby1
    - Continuous-Time Linear Systems: lti()
    - Discrete-Time Linear Systems
    - LTI Representations
    - Waveforms
    - Window functions
    - Wavelets
    - Peak finding
    - Spectral Analysis
- sparse: sparse matrices and associated routines
    - Contents
        - bsr_matrix: block sparse row matrix
        - coo_matrix: coordinate format
        - csr_matrix,, csc_matrix: compressed sparse row matrix
        - dia_matrix: sparse matrix with disgonal storage
        - dok_matrix: dictionary of keys based sparse matrix
        - lil_matrix: row-based linked list sparse matrix
        - spmatrix: 以上的所有类型
    - Functions
        - eye(): ones on diagonal
        - identity()
        - kron(A,B)
        - kronsum
        - diags(), spdiags
        - block_diag
        - tril
        - bmat: build,  hstack, vstack
        - rand: uniformly distributed values.  random()
    - Save and load sparse matrices
    - Sparse matrix tools: find(A)
    - identifying sparse matrices: issparse(x)
- spatial: spatial data structures and algorithms
    - Spatial Transformations
    - Nearest-neighbor Queries
        - KDTree
        - ckDTree
        - Rectangle: Hyperrectangel
    - Delaunay Triangulation, Convex Hulls and Voronoi Diagrams
    - Plotting Helpers
    - Simplex representation
- special special functions
- stats: statistical distributions and function
    - Statistical functions
    - Continuous distributions
    - Multivariate distributions
    - Discrete distributions
    - Summary statistics: descrube, gmean
    - Frequency statistics: cumfreq
    - Correlation functions
    - Statistical tests
    - Transformations

## 常用

- bsr_matrix: block sparse row matrix
        - coo_matrix: coordinate format
        - csr_matrix,, csc_matrix: compressed sparse row matrix
        - dia_matrix: sparse matrix with disgonal storage
        - dok_matrix: dictionary of keys based sparse matrix
        - lil_matrix: row-based linked list sparse matrix
        - spmatrix: 以上的所有类型