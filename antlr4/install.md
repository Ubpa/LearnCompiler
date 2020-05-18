# 安装

> windows

- 下载 Java

  > Java 分运行时环境 JRE 和开发工具 JDK，我们需要 JDK [->](https://www.oracle.com/java/technologies/javase-downloads.html) 

- 下载 antlr-4.7.1-complete.jar (或更新版本） （[下载链接](https://www.antlr.org/download/)），放到自己的 java 包路径，如 `D:/Program_Files/JavaLib` 

- 将 `D:/Program_Files/JavaLib/antlr-4.7.1-complete.jar` 添加到 `CLASSPATH` 系统环境变量中

- 设置命令缩写

  - 添加文件 `antlr4.bat` 和 `grun.bat` 放到 `D:/Program_Files/JavaLib` 中

    - `antlr4.bat` 

      ```bash
      java org.antlr.v4.Tool %*
      ```

    - `grun.bat` 

      ```bash
      @ECHO OFF
      SET TEST_CURRENT_DIR=%CLASSPATH:.;=%
      if "%TEST_CURRENT_DIR%" == "%CLASSPATH%" ( SET CLASSPATH=.;%CLASSPATH% )
      @ECHO ON
      java org.antlr.v4.gui.TestRig %*
      ```

  - 将 `D:/Program_Files/JavaLib` 加到系统环境变量 PATH 中

- 测试

  ```bash
  java org.antlr.v4.Tool
  java -jar D:/Program_Files/JavaLib/antlr-4.7.1-complete.jar
  ```

