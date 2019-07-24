---
layout:     post
title:      "数学数论算法"
subtitle:   "膜的新世界"
date:       2019-07-24 16:40:00 +0800
author:     "StephEric"
header-img: "img/post-bg-2015.jpg"
tags:
    - Maths
    - Algorithm
---


# 数学数论算法
---
[TOC]

# 数论入门

## 整除
- 整除性:$如果a能把b除尽，即b\% a=0, 则记为a\mid b$
- 自反性：$对于任意n,有n\mid n$
- 传递性：$若a\mid b 且 b\mid c，则a\mid c$

### 唯一分解定理（算数基本定理）

  1. 正整数的质因数分解是唯一的

  2. 正整数除法，就是把两个数的质因数分解，然后每个质数的指数相减

  3. $设A=2^{a_1}3^{a_2}5^{a_3}\cdots \ B=2^{b_1}3^{b_2}5^{b_3}\cdots$
	$A|B，当且仅当：a_i \leq b_i$

### 约数与倍数
- $[l,r]中k的倍数有(r-l+1)/k个$
- $O(n\log n)$求$d(n)$:
  引理：调和级数$
- $$1+\frac1 2 +\frac1 3+\cdots+\frac 1 n = ln\space n$$
  枚举$i \in [1,n],j \in N_+$,将$i* j\in [1,n]$的$d(i *j)$加一
  则时间复杂度为$O(n+\frac n 2 +\frac n 3+\cdots+\frac n n )=O(n\ln n)$
- 约数个数通过质因数分解解决，$d(n)=(r_1+1)(r_2 + 1)(r_3+1)\cdots$（乘法原理）

### 质数

- 一个整数无非平凡因子(除1和n外的因子)
- 质数有无限个

> 证明：
> 假设质数是有限个，设为$p_1,p_2\cdots p_n$
> 我们构造这样一个数$M$ $$M=p_1\times p_2 \times \cdots \times p_n + 1$$则$M$满足$M\%p_i=1(i\in [1,n])$
> $M$为质数，与假设矛盾，故假设不成立，命题得证

- 质数分布定理
  $$\lim_{n\rightarrow+\infty}\pi (n)=\frac n {\ln n}$$
- $O(\sqrt{n})$质因数分解
```cpp
// 将 x 分解质因数，结果从小到大储存在p[]中，返回质因子的个数
int factorize(int x,int p[]){
    int cnt=0;
    for(int i=2;i*i<=x;i++){
        while(x%i==0){
            p[++cnt]=i;
            x/=i;
        }
    }
    if(x>1) p[++cnt]=x;
    return cnt;
}
```
- 判断质数
```cpp
bool isprime(int x){
    if(x==1) return false;
    for(int i=2;i*i<=x;i++){
        if(x%i==0) return false;
    }
    return true;
}
```
- 质数筛法

### GCD和LCM

$$gcd(a,b)=gcd(b,a\%b)$$

> 证明：
> 设$r(a,b)$表示$a$,$b$所有公因数的集合
> 则$gcd(a,b)$是$r(a,b)$中的最大值
> **引理**：$a>b时,gcd(a,b)=gcd(a,a-b)=gcd(b,a-b)$
> 欲证引理，只需证$r(a,b)=r(a,a-b)=r(b,a-b)$
> $设 a=p\cdot d,b=q\cdot d,则 a-b=(p-q)\cdot d (p,q\in Z,d\in r(a,b) )$
> 显然，引理得证
> $\because gcd(a,b)=gcd(b,a-b)$
> $\therefore gcd(a,b)=gcd(b,a\%b)$

> 时间复杂度最坏情况为$O(\log n),条件是a,b为Fib数列的相邻项$

$$gcd(n,m)=\prod p_i^{min(a_i,b_i)}$$

$$lcm(n,m)=\prod p_i^{max(a_i,b_i)}$$

$$gcd(n,m) \cdot lcm(n,m)= n\cdot m$$

$$gcd(F[a],F[b])=F[gcd(a,b)]$$

> **引理1** $\gcd(F[n+1],F[n])=1$
> 证明： 根据辗转相减法则 
> $\gcd(F[n+1],F[n]) \\
> =\gcd(F[n+1]-F[n],F[n]) \\
> =\gcd(F[n],F[n-1]) \\
> =\gcd(F[2],F[1]) \\
> =1$ 

> **引理2** $F[m+n]=F[m-1]F[n]+F[m]F[n+1]$
> 证明： 
> $F[n+m] \\
> =F[n+m-1]+F[n+m-2] \\
> =2*F[n+m-2]+F[n+m-3] \\
> =\cdots$
> 设 
> $F[n+m] \\
> =a[x]*F[n+m-x]+b[x]*F[n+m-x-1]\\
> =a[x]*(F[n+m-x-1]+F[n+m-x-2])+b[x]*(F[n+m-x-1)\\
> =(a[x]+b[x])*F[n+m+x-1]+a[x]*F[n+m+x-2]$
> $当x=1时有 a[1]=F[2],\  b[1]=F[1]$
> $当x=2时有 a[2]=F[2]+F[1]=F[3],\ b[2]=a[1]=F[2] $
> $当x=k+1时有 a[k+1]=a[k]+b[k]=F[k+1]+F[k]=F[k+2] \\b[k+1]=a[k]=F[k+1] $
> $所以当x=n时有 $
> $F[n+m]=a[n]F[m]+b[n]F[m-1]\\ 
> =F[n+1]F[m]+F[n]F[m-1] $

> **引理3** $\gcd(F[n+m],F[n])=\gcd(F[n],F[m]) $
> 证明： 
> $\gcd(F[n+m],F[n]) \\
> =\gcd(F[n+1]F[m]+F[n]F[m-1],F[n])\\
> =\gcd(F[n+1]F[m],F[n])\\
> =\gcd(F[n+1],F[n])\cdot \gcd(F[m],F[n]) \\
> =\gcd(F[m],F[n])$

> 于是显然有 $\gcd(F[n],F[m])=F[\gcd(n,m)]$

## 模　

- 两个符号

  - 上取整 ceil() $\lceil \frac a b \rceil$

    - 下取整 floor() $\lfloor \frac a b \rfloor$

- 随时取模

  - 可证：各位数字之和能被3整除则该数能被3整除

    > 考虑三位数的$n$
    > 设$n=\overline{abc}$,则有
    > $n\% 3=(100a+10b+c)\% 3 \\ =[(100\%3)a+(10\%3)b+c]\%3 \\ =(a+b+c)\%3$
    > 其余位数同理

  - 可证：最后两位数能被4整除则该数能被4整除

    > 考虑四位数的$n$
    > 设$n=\overline{abcd}$,则有
    > $n\%4=(1000a+100b+10c+d)\%4 \\ =[(1000\%4)a+(100\%4)b+10c+d]\%4 \\ =(10c+d)\%4 \\ =\overline{cd}\%4$
    > 其余位数同理


$$(a+b)\%\ p=(a\% p+b\%p)\%p$$ $$(a\times b)\%p=(a\%p \times b\%p)\%p$$

- 模意义

  - $36\equiv11(mod5)$	


$$a+c\equiv b+c (mod\ p)$$ $$a\cdot c\equiv b\cdot c (mod\ p)$$
    
- 模环

- 奇怪的性质

$$\lfloor\frac{\lfloor\frac{a}{b}\rfloor}{c}\rfloor= \lfloor{\frac{a}{bc}}\rfloor=\lfloor\frac{\lfloor\frac a c \rfloor}b\rfloor$$

> 证明：
> 设$kbc \leq a < (k+1)bc , k \in N$
> 因此 $$kc \leq \frac a b < (k+1)c \\ k \leq \frac a {bc} < k+1$$因此
> $$\lfloor \frac a {bc} \rfloor = k$$ $由于kc与(k+1)c都是整数，所以$ 
> $$kc \leq \lfloor \frac a b \rfloor ＜ (k+1)c$$因此
> $$k \leq \frac {\lfloor \frac a b \rfloor} c < k+1$$因此
> $$\lfloor \frac {\lfloor \frac a b \rfloor} c \rfloor = k = \lfloor \frac a {bc} \rfloor$$同理
> $$\lfloor \frac {\lfloor \frac a c \rfloor} b \rfloor = k = \lfloor \frac a {bc} \rfloor$$即
> $$\lfloor\frac{\lfloor\frac{a}{b}\rfloor}{c}\rfloor= \lfloor{\frac{a}{bc}}\rfloor=\lfloor\frac{\lfloor\frac a c \rfloor}b\rfloor$$

典型的质数：19260817 998244353

# 数论进阶

## 线性不定方程

### 二元一次不定方程

- 裴蜀定理：关于整数$x$,$y$的不定方程$ax+by=\gcd(a,b)$必定有解

- 处理不定方程$ax+by=c$
    - $c=\gcd(a,b)$，此时据裴蜀定理有解
    - $c=k\cdot \gcd(a,b)$方程两边除以$k$
    - $c \perp \gcd(a,b)$此时无解

- 扩展欧几里得：已知$bx+(a\%b)y=\gcd(a,b)$的解$(x,y)$，推出$ax'+by'=\gcd(a,b)$的解$(x',y')$ $$x'=y,\ y'=x-\lfloor \frac a b \rfloor \cdot y$$

```cpp
int exgcd(int a,int b,int& x,int& y){
    if(b==0){
        x=1;y=0;
        return a;    
    }
    int tmp=exgcd(b,a%b,x,y);
    int t=x;
    x=y;
    y=t-a/b*y;
    return tmp;
}
```

### 模线性同余方程组

考虑方程组
$$\begin{cases}
 \ x \equiv a_1 (mod \ p_1) \\
 \ x \equiv a_2 (mod \ p_2) \\
 \hspace{10pt} \cdots \\
 \ x \equiv a_n (mod \ p_n) \\
\end{cases}$$
满足$p_1,p_2,\cdots,p_n$两两互质,求$x$

> 我们需要构造一个$x=\sum r_i , r_i$应满足
> $$r_i \% p_k= 
> \begin{cases}0,\qquad i\ne k\\ 
>  a_i,\quad\ \ i=k
> \end{cases}$$
> 我们设$P=\prod p_i$
> 显然$$r_i=a_i\frac P p_i \cdot inv_{p_i}(\frac P p_i)$$

中国剩余定理（CRT）：
模线性同余方程组的解系为
$$ans\% \prod_{i=1}^n p_i=\sum_{i=1}^n a_iP_iinv_{p_i}(P_i)\quad其中P_i= \frac {\prod_{j=1}^n p_j} {p_i}$$

## 逆元

- $x\cdot inv(x) \equiv 1 (mod\ p)$

- 费马小定理： $n^{p-1} \equiv 1 (mod\ p)$

- 模质数意义下的逆元求法：$inv(x)=x^{p-2}\  mod\ p$

# 组合数学

## 加法原理与乘法原理

乘法原理中，各步骤是独立的；
加法原理中，各手段只能用其中一个。

## 排列与组合

- 排列符合乘法原理$$A_n^m=n(n-1)(n-2)\cdots (n-m+1)=\frac{n!}{(n-m)!}$$

- 组合即为排列去除掉重复情况$$C_n^m=\frac{A_n^m}{A_m^m}=\frac{n!}{m!(n-m)!}$$

- 组合数递推(Pascal公式)：当选完前n-1个数后，对于第n个数要么选要么不选。符合加法原理$$C_n^m=C_{n-1}^m+C_{n-1}^{m-1}$$

- Lucas定理（复杂度$O(\log n)$）$$C_n^m=C_{n/p}^{m/p}\cdot C_{n\%p}^{m\%p}\quad (mod\ p)$$ 

# 数学漫谈