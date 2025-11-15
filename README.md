下面给你一份 **完整、清晰、可直接使用** 的命令行总结，用于 Project3 的编译、运行、测试 —— **完全针对你当前的 Linux + VSCode + branch1 结构**。

---

# ✅ 一次性总结：Project3 命令行流程

以下所有命令都必须在 **项目根目录** 执行（里面有 `src/`, `libs/`, `Splc.g4`, `testcases/`）。

---

# 1️⃣ 第一次使用必须执行：生成 ANTLR 的 Java 源码

只需要做 **一次**（除非你改了 Splc.g4）：

```bash
mkdir -p src/main/java/generated/Splc

java -jar libs/antlr-4.13.2-complete.jar \
  -Dlanguage=Java \
  -visitor \
  -package generated.Splc \
  -o src/main/java/generated/Splc \
  Splc.g4
```

执行后会生成：

```
src/main/java/generated/Splc/
    SplcLexer.java
    SplcParser.java
    SplcBaseVisitor.java
    ...
```

---

# 2️⃣ 编译所有 Java 文件（每次修改 `.java` 后都要执行）

```bash
javac -cp libs/antlr-4.13.2-complete.jar \
      -d out \
      $(find src/main/java -name "*.java")
```

编译成功不会有输出。
如果有报错，你需要修代码再重新执行此命令。

---

# 3️⃣ 运行程序

标准运行命令：

```bash
java -cp "out:libs/antlr-4.13.2-complete.jar" Main
```

* 必须包含 `out`（编译后的 class 文件）
* 必须包含 ANTLR 的 jar

---

# 4️⃣ 运行不同 testcases

## 方法 A：修改 Main.java 里的路径

例如：

```java
InputStream input = new FileInputStream("testcases/project3/ok_02.splc");
```

然后重新编译：

```bash
javac -cp libs/antlr-4.13.2-complete.jar -d out $(find src/main/java -name "*.java")
```

运行：

```bash
java -cp "out:libs/antlr-4.13.2-complete.jar" Main
```

---



# 5️⃣ 清理旧的编译结果（如果需要）

```bash
rm -rf out
mkdir out
```

然后重新编译：

```bash
javac -cp libs/antlr-4.13.2-complete.jar -d out $(find src/main/java -name "*.java")
```

---

# 6️⃣ 检查 Main.class 是否唯一（排查错跑旧文件问题）

```bash
find . -name "Main.class"
```

应该只看到：

```
./out/Main.class
```

