# YGR #4 校内胡策
## A 胡策的连通块
### Description
胡策在纸上画出了一张完全图(无重边和自环), 点的编号为$1\sim n$, 每一个点都有一个权值$w_i$.

胡策规定, 这张图上一个连通块的贡献为它所包含的点的点权最大值, 而这张图的总贡献就是图上**所有不同的连通块**的贡献之和.

请帮助胡策求出这个贡献.

$$\rm \color{red}{Warning:}$$
1. 在本题中, $1$个点也可以被视为一个连通块.
2. 两个连通块$u = (V_1,E_1), v = (V_2,E_2)$被定义为**不同的**, 当且仅当$\exist k \in V_1$且$k \not \in V_2$

### Input
第一行, 一个正整数$n$, 表示完全图点的数量

第二行, $n$个正整数, 第$i$个正整数$w_i$表示$i$号点的点权.

### Output
一个正整数, 表示这张图的贡献.

由于这个数可能很大, 请把他对$998244353$取模的结果输出即可.

### Samples
#### Input #1
```markdown
3
1 2 3
```
#### Output #1
```markdown
17
```

### Hint
- $\rm Subtask1:$ 对于$10\%$的数据, $1 \leq n \leq 20$
- $\rm Subtask2:$ 对于$30\%$的数据, $1 \leq n \leq 10^3$
- $\rm Subtask3:$ 对于$100\%$的数据, $1 \leq n \leq 10^6$

### Limits
- $\rm Time \; Limit:$ $1000ms$
- $\rm Memory \; Limit:$ $128 \rm MB$

## B 胡策的足球赛

## C 胡策的矩阵树
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjMzMjEzNTEzXX0=
-->