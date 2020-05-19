# 词法分析

## 2.1 词法记号及属性

词法记号、模式、词法单元

|   记号   | 词法单元示例 | 解释 |
| ---- | ---- | ---- |
| if | if | `'i'` + `'f'` |
| for | for | `'f'`+`'o'`+`'r'` |
| relation | <,<=,=,... |      |
| id | sum, count |      |
| number | 3.1 |      |
| literal | "hello" |      |
| whitespace | \n |      |

> whitespace 无意义，被丢弃，不提供给词法分析器

- 关键字 keyword：有专门的意义和用途，如 `if`,`for` 
- 保留字：有专门的意义，不能当做一般的标识符使用

> C 的关键字是保留字

词法分析

- 从做到右读取输入串，每次识别出一个 token
- 可能需要 lookahead 来判断当前是否是一个 token 的结尾、下一个 token 的开始

词法分析器

- 识别子串并对应到 token

- 返回 token 的值或**词法单元** lexeme

  > 词法单元是子串

> 示例
>
> ![image-20200518212511817](assets/02_lex/image-20200518212511817.png)

术语

- 字母表：符号的有限集合
- 串：符号的有穷序列（$\epsilon$ 表示空串）
- 语言：字母表上的串集
- 句子：属于语言的串

## 2.2 词法记号的描述与识别

串运算

- 连接（积）：$xy$，$s\epsilon=\epsilon=s$ 
- 幂：$s^0$ 为 $\epsilon$，$s^i=s^{i-1}s(i\ge 0)$ 

> 群

语言运算

- 并
- 连接：$LM=\{st|s\in L, t\in M\}$ 
- 幂：$L^0=\{\epsilon\}$，$L^i=L^{i-1}L$ 
- 闭包：$L^*=L^0\cup L^1\cup L^2\cup \dots$ 
- 正闭包：$L^+=L^1\cup L^2\cup \dots$ 

正规式 regular expression

正规式用来表示简单的语言，叫做**正规集** 

| 正规式     | 语言            |
| ---------- | --------------- |
| $\epsilon$ | $\{\epsilon\}$  |
| $a$        | $\{a\}$         |
| $(r)$      | $L(r)$          |
| $(r)|(s)$  | $L(r)\cup L(s)$ |
| $(r)(s)$   | $L(r)L(s)$      |
| $(r)^*$    | $(L(r))^*$      |

正规定义 regular definition

- 对正规式命名，使正规式表示简洁：$d_i\to r_i(i=1,\dots,n)$，且 $d_i\neq d_j(i\neq j)$ 
- $r_i$ 是 $\Sigma\cup\{d_1,\dots,d_{i-1}\}$ 上的正规式

词法记号的识别：转换图

![image-20200518215241593](assets/02_lex/image-20200518215241593.png)

歧义

- 最长匹配规则：lookahead，不符合则回退
- 优先原则：同时匹配时优先选择先列出的

## 2.3 有限自动机

不确定的有限自动机

> nondeterministic finite automaton, NFA

数学模型

- 有限的状态集合 $S$ 
- 输入符号集合 $\Sigma$ 
- 转换函数 $\mathop{move}:S\times(\Sigma\cup\{\epsilon\})\to P(S)$ 
- 状态 $s_0$ 是唯一的开始状态
- $F\subseteq S$ 是接受状态集合

> 同时有多种状态

确定的有限自动机

> deterministic finite automaton, DFA

- 有限的状态集合 $S$ 
- 输入符号集合 $\Sigma$ 
- 转换函数 $\mathop{move}:S\times(\Sigma\cup\{\epsilon\})\to S$，且可以是部分函数
- 状态 $s_0$ 是唯一的开始状态
- $F\subseteq S$ 是接受状态集合

NFA 与 DFA 之间的变换

> PPT 在此处不够详细

子集构造法

- $\epsilon$-闭包：状态 $s$ 的 $\epsilon$-闭包是 $s$ 经过 $\epsilon$ 转换所能到达的状态集合
- NFA 的初始状态的 $\epsilon$ 闭包对应于 DFA 的初始状态

不一定得到最简 DFA

可区别状态 distinguishable state

分别从 $s$ 和 $t$ 出发，若存在 $w$，使得其一到达接受状态，另一到达非接受状态，则为可区别状态，否则为不可区别状态，故可合并

## 2.4 从正规式到有限自动机

正规式->NFA->DFA->化简的DFA

采用语法制导 syntax-driven 的算法来构造 NFA

![image-20200518222825894](assets/02_lex/image-20200518222825894.png)

![image-20200518222832154](assets/02_lex/image-20200518222832154.png)

![image-20200518222855479](assets/02_lex/image-20200518222855479.png)

![image-20200518222902880](assets/02_lex/image-20200518222902880.png)

![image-20200518222945563](assets/02_lex/image-20200518222945563.png)

性质

![image-20200518223000347](assets/02_lex/image-20200518223000347.png)

![image-20200518223009568](assets/02_lex/image-20200518223009568.png)

