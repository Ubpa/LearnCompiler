# 03. 语法分析

## 3.1 上下文无关文法

正规式不能描述**配对**或**嵌套**的结构

上下文无关文法 Context-free Grammar (CFG) 是四元组 $(V_T,V_N,S,P)$ 

- $V_T$：终结符（terminal，记号 token 的第 1 元）集合
- $V_N$：非终结符 nonterminal 集合
- $S$：开始符号 start symbol，是一个非终结符
- $P$：产生式 production 集合

> 示例
> $$
> E\to EAE|(E)|-E|id\\
> A\to +|*
> $$

推导 derivation

把产生式看成是重写规则，把符号串中的非终结符用其产生式右部的串来代替

> 示例
> $$
> E\Rightarrow-E\Rightarrow-(E)\Rightarrow-(E+E)\Rightarrow -(id+E)\Rightarrow -(id+id)
> $$

推导结果（不含非终结符的串）为**实例** 

 记法

- 0 步或多步推导：$S\Rightarrow^*\alpha$ 
- 多步推导：$S\Rightarrow^+\alpha$ 

上下文无关语言

由上下文无关文法 G 产生的语言：从开始符号 $S$ 出发，经 $\Rightarrow^+$ 推导所能到达的所有仅由终结符组成的串

句型 sentential form：$S\Rightarrow^*\alpha$，$S$ 是开始符号，$\alpha$ 是由终结符和或非终结符组成的串，则 $\alpha$ 是文法 $G$ 的句型

句子 sentence：仅由终结符组成的句型

最左推导（leftmost derivation）：每步代换**最左边**的非终结符

最右推导（rightmost or canonical，规范推导）：每步代换**最右边**的非终结符

分析树 parse tree

> 示例
>
> $-(id+id)$ 最左推导分析树
>
> ![image-20200519144308662](assets/03_parsing/image-20200519144308662.png)

二义文法：句子有不止一种最左推导

> 示例
>
> $id*id+id$ 有两棵不同的最左推导分析树
>
> ![image-20200519144425850](assets/03_parsing/image-20200519144425850.png)

## 3.2 语言和文法

CFG 可表示正规式：正规式转NFA，每状态引入非终结符，每条弧对应于产生式的一个分支

> 示例
>
> 正规式：$(a|b)^*ab$ 
>
> NFA
>
> ![image-20200519145054800](assets/03_parsing/image-20200519145054800.png)
>
> CFG
> $$
> \begin{align}
> A_0&\to aA_0|bA_0|aA_1\\
> A_1&\to bA_2\\
> A_2&\to\epsilon
> \end{align}
> $$
>
> > 其中 $A_2\to\epsilon$ 并不必要，可代换到其他产生式中从而消掉

词法分析：DFA

语法分析：CFG

验证文法产生的语言：归纳法（按步或按串长）

无二义的文法

> 示例
> $$
> \begin{align}
> expr&\to expr+term|term\\
> term&\to term*factor|factor\\
> factor&\to id|(expr)
> \end{align}
> $$

消除二义性

- 消除左递归

  - 左递归：$A\Rightarrow^+ A\alpha$ 

  - 自上而下的分析不能用于左递归文法

  - 直接左递归：某分支开头为同于左边的非终结符

  - 消除直接左递归

    > 示例
    >
    > $A\to A\alpha|\beta$ 改为
    > $$
    > \begin{align}
    > A&\to\beta A^\prime\\
    > A^\prime&\to\alpha A^\prime|\epsilon
    > \end{align}
    > $$

  - 间接左递归：2步及以上推导后出现开头为同于左边的非终结符的句型

  - 消除间接左递归：多步代换直至出现直接左递归，然后再消除直接左递归

- 提左因子

非上下文无关的语言

> 示例
>
> - $L_1=\{wcw|w\in(a|b)^*|\}$：用来抽象标识符的声明应先于其引用
>
>   > C、Java 不是上下文无关语言
>
> - $L_2=\{\alpha^nb^mc^nd^m|n\ge 0,m\ge 0\}$：用来抽象形参个数和实参个数应相同
>
> - $L_3=\{a^nb^nc^n\}$：用来抽象早先排版描述的一个现象（字母、回退、下滑线）
>
> 但稍微改动一下可以变成上下文无关语言
>
> - $L_1^\prime=\{wcw^R|w\in(a|b)^*\}$（$w^R$ 是 $w$ 的回文）
> - $L_2^\prime=\{a^nb^mc^md^n|n\ge 1,m\ge 1\}$ 

## 3.3 自上而下分析

为输入串寻找最左推导：试探-回溯

不能处理左递归文法

LL(1) 文法

> scanning from left ot right, leftmost derivation, ahead 1

文法函数

- $FIRST(\alpha)=\{a|\alpha\Rightarrow^*a\dots,a\in V_T\}$，若 $\alpha\Rightarrow^* \epsilon$，则 $\epsilon \in FIRST(\alpha)$ 
- $FOLLOW(A)=\{a|S\Rightarrow^*\dots Aa\dots,a\in V_T\}$，如果 $A$ 是某个句型的最右符号，那么 $\$$ 属于 $FOLLOW(A)$ 

计算 $FIRST(X),X\in V_T\cup V_N$ 

- $X\in V_T$：$FIRST(X)=\{X\}$ 
- $X\in V_N$ 且 $X\to Y_1Y_2\dots Y_k$ 
  - 若 $\epsilon\in FIRST(Y_1),\dots,\epsilon\in FIRST(Y_{i-1})$ 中，则 $FIRST(Y_i)\in FIRST(X)$ 
  - 如果  $\epsilon\in FIRST(Y_1),\dots,\epsilon\in FIRST(Y_k)$ 中，则将 $\epsilon$ 加到 $FIRST(X)$ 
- $X\in V_N$ 且 $X\to \epsilon$：将 $\epsilon$ 加入到 $FIRST(X)$ 中

计算 $FIRST(X_1 X_2\dots X_n),X_i\in V_T\cup V_N$，包含

- $FIRST(X_1)$ 的所有非 $\epsilon$ 符号
- $FIRST(X_i)$ 的所有非 $\epsilon$ 符号，如果 $\epsilon$ 在 $FIRST(X_1)\dots FIRST(X_{i-1})$ 中，则
- $\epsilon$，如果 $\epsilon$ 在 $FIRST(X_1)\dots FIRST(X_n)$ 中

计算 $FOLLOW(A),A\in V_N$，

- $\$$ 加入到 $FOLLOW(S)$ 中
- 如果 $A\to\alpha B\beta$，则 $FIRST(\beta)$ 加入到 $FOLLOW(B)$ 中
- 如果 $A\to\alpha B$ 或 $A\to \alpha B\beta$ 且 $\epsilon \in FIRST(\beta)$，则 $FOLLOW(A)$ 的所有符号加入到 $FOLLOW(B)$ 中

> 计算 $FOLLOW(A)$ 的过程不是很直接

LL(1) 文法的定义

任何两个产生式 $A\to\alpha|\beta$ 满足

- $FIRST(\alpha)\cap FIRST(\beta)=\empty$ 
- 若 $\beta\Rightarrow^* \epsilon$，则 $FIRST(\alpha)\cap FOLLOW(A)=\empty$ 

LL(1) 性质

- 没有公共左因子
- 不是二义的
- 不含左递归

递归下降的预测分析

- 为每一个非终结符写一个分析过程
- 这些过程可能是递归的

预测分析表

- 行：非终结符
- 列：终结符或 $\$$ 
- 单元：产生式

> 示例
>
> ![image-20200519171435036](assets/03_parsing/image-20200519171435036.png)
>
> 执行结果（部分）
>
> ![image-20200519171457768](assets/03_parsing/image-20200519171457768.png)

构造方法

![image-20200519171533967](assets/03_parsing/image-20200519171533967.png)

*错误恢复

## 3.4 自下而上分析

> 移进-归约分析

归约：把输入串约成文法的开始符号，是最右推导的逆过程

![image-20200519173520356](assets/03_parsing/image-20200519173520356.png)

操作：移进，归约

冲突

- 移进归约冲突
- 归约归约冲突

## 3.5 LR 分析器

> scanning from left to right, right most derivation in reverse

![image-20200519181210404](assets/03_parsing/image-20200519181210404.png)

活前缀（viable prefix）

右句型的前缀，不超过最右句柄 $w$ 的右端
$$
S\Rightarrow_{rm}^* \gamma A w\Rightarrow_{rm}\gamma \beta w
$$
$\gamma \beta$ 的任何前缀都是活前缀，$w$ 仅包含终结符

LR 分析模型栈底到栈顶的文法符号连接形成的串是活前缀，缓冲区中剩余的记号串为 $w$ 

LR 文法：所有存在条目都唯一的 LR 分析表

action+goto：本质上是识别活前缀的 DFA

栈顶的状态符号包含了确定句柄所需要的一切信息

已知的最一般的无回溯的移进-归约方法

能分析的文法类时预测分析能分析的文法类的真超集

能及时发现语法错误

手工构造分析表的工作量太大

*SLR 分析表的构造

