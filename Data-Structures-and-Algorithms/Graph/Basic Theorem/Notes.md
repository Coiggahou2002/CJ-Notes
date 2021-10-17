# 📔 基本概念

## 一、图的表示

$G=(V,E)$，$V$为顶点集，$E$为边集

常将顶点数为 $p$ ，边数为 $q$ 的图简称为 $(p,q)$ 图

在没有特殊说明时，$G$ 指无向图，$D$ 指有向图

## 二、图的基本信息描述

### 边

无向图中的边用无序对表示，如 $(u,v)$

有向图中的边用有序对表示，如 $<u,v>$ 表示从 $u$ 到 $v$ 的有向边

### 阶

图的阶数就是图中顶点的总个数

### 度

#### 对于无向图的顶点

一个顶点的度，指的是该顶点直接相连的边的数目，记作 $deg(u)$

#### 对于有向图的顶点

入度：与该顶点直接相连的“入边”的数目，记 $deg^{-}(u)$

出度：与该顶点直接相连的“出边”的数目，记 $deg^{+}(u)$

度：该顶点入度与出度的和

#### 最大度与最小度

$\delta(G)$  ：所有顶点度数中的最小值

$\Delta(G)$ ：所有顶点度数中的最大值

相应地，加上符号，如 $\delta ^{+}(G)$ ， 表示有向图的入度、出度的最大最小值

#### 特殊度数的点

**孤立点**：度数为 0 的顶点

**悬挂点**：度数为 1 的顶点

**奇度顶点**：度数为奇数的顶点，简称奇点

**偶度顶点**：度数为偶数的顶点，简称偶点

#### 度数列

顶点标定的无向图，按标定顺序列出每个顶点的度数，即是该图的度数列（唯一）.

#### 度满足的基本关系

$$
0\leq deg(u)\leq p-1
$$

### 邻域

对于无向图 $G=(V,E)$ ，$\forall u\in V$ ，$u$ 的邻域 $N_G(u)$ 指的是与 $u$ 直接相连的所有顶点的集合，$u$ 的闭邻域 $\bar{N}_G\left( u \right) $ 则是邻域再加上 $u$ 本身。

$N_G\left( u \right) =\left\{ v|v\in V\land \left( u,v \right) \in E\land u\ne v \right\} $

$\bar{N}_G\left( u \right) =N_G\left( u \right) \cup \left\{ u \right\} $

对于有向图 $D=(V,E)$ ，它的邻域就是先驱和后继元集的并集，闭邻域定义与无向图类似，加上自己即可。

### 先驱和后继元集

这是有向图专用概念。对于有向图 $D=(V,E)$，$\forall u\in V$ ，$u$ 的先驱元集指的是直接通过有向边指向 $u$ 的所有顶点的集合，后继元集则指的是 $u$ 通过有向边直接指向的所有顶点的集合。

先驱元集：$\varGamma _{D}^{-}\left( v \right) =\left\{ u|u\in V\land <u,v>\in E\land u\ne v \right\} $

后继元集：$\varGamma _{D}^{+}\left( v \right) =\left\{ u|u\in V\land <v,u>\in E\land u\ne v \right\} $

### 边与点的关系

#### 关联

若 $e_k=(v_i,v_j)$ 或 $e_k=<v_i,v_j>$ ，称 $e_k$ 与 $v_i(v_j)$ 关联，即**边与点关联**。

#### 相邻

点相邻：两顶点之间有边连接，则称这两个顶点相邻

边相邻：两条边中，一条边的终点是另一条边的起始点，则这两条边相邻

#### 平行边

无向图中关联两个顶点的边如果多于 1 条，则这些边都称为平行边，平行边的条数称为**重数**。

## 三、图的种类

### 平凡图

只有一个顶点的图

### 空图

$V(G)=\varnothing$，即一个顶点都没有的图

### 零图

$|E(G)|=0$，即没有边的图，$n$ 阶零图记作 $N_n$

### 基图

基图的概念附属于有向图。将给定的有向图的所有边去掉方向之后，得到的便是该有向图的基图。

### 简单图

不含平行边也不含环的图

### 多重图

含平行边的图

### $r$-正则图

所有顶点的度都等于 $r$ 的图

### 完全图 $K_n$

给定图 $G=(V,E)$ ，每个顶点都与其余 $(n-1)$ 个顶点相连，这样的图称为 $n$ 阶完全图，即 $K_n$

 $n$ 阶完全图的总边数为 $\frac{n(n-1)}{2}$

### 圈图 $C_n$

### 轮图 $W_n$

圈图中央加一个“总站”

## 四、路

### 通路

图中任一顶点与边的交替序列称为通路

### 回路

起点和终点相同的通路

### 简单通路

边不重复的通路

### 复杂通路

边有重复的通路

### 简单回路

起点和终点相同的简单通路

### 复杂回路

边有重复的回路

### 初级通路 / 路径

点不重复、边也不重复的通路

### 初级回路 / 圈

起点和重点相同的路径。长度为奇数的圈称**奇圈**，否则称**偶圈**。

> 长为 1 的圈只能是环，长为 2 的圈只能由平行边生成，因此圈的长度至少为 3.

### 欧拉通路

不重复地遍历每一条边（顶点可以重复）的通路

### 欧拉回路

起点和终点相同的欧拉通路

### 哈密顿通路

遍历每个顶点且不走重复边的通路

### 哈密顿回路

起点和终点相同的哈密顿通路

## 五、图的关系

### 子图

$V'\subseteq V \and E'\subseteq E$，则 $G'$ 为 $G$ 的子图（先从 $G$ 的点集中挑若干个点组成新的点集，再**从原图中与这些点关联的**边集中挑若干条边，这样生成的新图 $G'$ 就是子图）

### 母图

子图的相对概念

### 真子图

$V'\subset V \and E'\subset E$（改成真子集），其余同子图概念定义

### 生成子图

$V' = V \and E'\subseteq E$，简单来讲，即顶点数不减少的子图

### 导出子图

#### 1. 由点集导出的子图

设 $V'\subseteq V$，$\forall u,v\in V'$ ，若 $(u,v)\in E$，则有 $(u,v)\in E'$，并称 $G'=(V',E')$ 为 $G$ 由 $V'$ 导出的子图. 

即：从原图中挑若干个点作为导出子图的点集，将新点集中的点两两配对，检查每个点对的边是否在原图中有，如果有，那导出子图也必须有.

#### 2. 由边集导出的子图

设 $E'\subseteq E$，$\forall (u,v)\in E'$，若 $u,v\in V$，则有 $u,v\in V'$，并称 $G'=(V',E')$ 为 $G$ 由 $E'$ 导出的子图.

即：从原图中挑若干条边作为导出子图的边集，新边集中关联到的所有点，导出子图必须有.



# 🔒 重要定理

## 握手定理

给定**无向图** $G=(V,E)$ ，所有顶点的度数之和等于总边数的一半。换句话说，每条边对度数总和的贡献为 2。
$$
\sum_{u\in V}^{}{deg\left( u \right) =2q}
$$
有向图也满足上述结论，并且还满足所有顶点的入度之和等于出度之和（所谓有入必有出）。
$$
\sum_{u\in V}^{}{deg^{-}\left( u \right)} =\sum_{u\in V}^{}{deg^{+}\left( u \right)} = q
$$

## 握手定理推论

任何图中，奇度顶点的个数必定为偶数.

**证明简述**

给定一个图 $G=(V,E)$ ，将点集 $V$ 按度数分成 $V_{奇}$ 和 $V_{偶}$ 两部分，由握手定理可知，所有顶点度数之和为边数的两倍，即所有顶点的度数和为偶数，而属于 $V_{偶}$ 这部分顶点的度数和必然为偶数（若干个偶数和必为偶数），那么想要让总度数和为偶数的话，$V_{奇}$ 部分的所有顶点度数和必须为偶数，而因为 $V_{奇}$ 部分的每个顶点度数都是奇数，所以 $|V_{奇}|$ 就必须是偶数（偶数个奇数的和才能是偶数），结束。

## 可图化判定

给定一个度数列，它是可图化的，当且仅当 $\sum_{i=1}^{p}deg(u_i)$ 为偶数

## 判定简单图的必要条件

$\Delta(G)\le n-1$

## Ramsey 定理

6 个人当中，或有 3 个人互相认识，或有 3 个人互相不认识

## 连通性判定

## 欧拉回路的判定

含有至少 2 个顶点的**连通**多重图具有欧拉回路，当且仅当它的奇度顶点个数为 0.

## 欧拉通路的判定

连通多重图具有欧拉通路但无欧拉回路，当且仅当它恰有 2 个奇度顶点.

## 哈密顿回路的判定

### 1.完全图的哈密顿回路

当 $n\ge3$ 时，$K_n$ 有哈密顿回路

### 2.欧尔定理

如果 $G$ 是有 $n$ 个顶点的简单图，且 $n\ge3$ ，并且对于 $G$ 中每一对不相邻的顶点 $u$ 和 $v$ 来说，都有
$$
\deg(u)+\deg(v)\ge n
$$
则 $G$ 有哈密顿回路.

### 3.狄拉克定理

如果 $G$ 是有 $n$ 个顶点的简单图，且 $n\ge3$ ，并且 $G$ 中每个顶点的度都至少为 $\lceil \frac{n}{2} \rceil $ ，则 $G$ 有哈密顿回路. 

> 个人理解：欧尔定理要求在图中随便挑两个不相邻顶点，它们“配合”的度数要足够大，而狄拉克定理可看作是将这种“配合”平均分担到每一个顶点身上，算是欧尔定理的推论.

## 哈密顿回路的否定

如果图中存在度为 1 的顶点，则该图必定没有哈密顿回路.



# 💡 经典问题

## 中国邮递员问题

在图中找出一条回路，使得该回路以最少的边数（或者边权之和最小）至少遍历每条边一次.

## 旅行商问题 Traveling Salesman Problem

一个旅行商人想要拜访 $n$ 个城市，每个城市只能拜访 1 次（除出发点外），最后回到出发的城市，要求找出一条路径，使得路径总和最小.