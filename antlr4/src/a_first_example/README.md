# A First Example

编写语法 grammar 文件 [Hello.g4](Hello.g4) 

```c
// Define a grammar called Hello
grammar Hello;
r  : 'hello' ID ;         // match keyword hello followed by an identifier
ID : [a-z]+ ;             // match lower-case identifiers
WS : [ \t\r\n]+ -> skip ; // skip spaces, tabs, newlines
```

然后执行命令

```bash
antlr4 Hello.g4
javac Hello*.java
```

然后执行命令

```bash
grun Hello r -tree
```

然后再命令框中输入

```bash
hello ubpa
```

然后按 `ctrl+z`（cmd 中会显示 `^Z`），回车，得到结果

```bash
(r hello ubpa)
```

这是 LISP 记号，`r` 是根节点， `hello` 是左节点，`ubpa` 是右节点

还有 GUI 模式，如下命令

```bash
grun Hello r -tree
```

然后再命令框中输入

```bash
hello ubpa
```

然后按 `ctrl+z`（cmd 中会显示 `^Z`），回车，得到图片结果

