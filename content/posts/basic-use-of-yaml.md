---
title: YAML的基本用法
date: 2019-05-01T13:29:22+08:00
categories: 
- 笔记
tags: 
- YAML
---



块序列（Block-Sequence）用**破折号**和**空格**来表示。

```yaml
- Item1
- Item2
```

键值对（Key-Value pair）用**冒号**和**空格**来表示。

```yaml
date: 2019
count: 2
```

注释以**井号**开始。

```yaml
# Comment
```

YAML 使用特定的标识符（而不是缩进）来表示范围。

流序列用**方括号**表示，序列中的项用**逗号**和**空格**分开。

```yaml
- [item1, item2, item3]
```

与流序列语法相似，流映射使用**花括号**表示。

```yaml
Tom: {
  age: 19,
  sex: male
}
```

YAML 使用**三个破折号**将指令从文档内容中分隔开。如果没有指令，这会被当做是文档开始的信号。**三个点**表示文档结束，之后没有新文档的开始，这一特性通常是在通信频道使用。

```yaml
# Level 1
---
Jack
Bill
Sam

# Level 2
---
Smith
Pom
```

```yaml
---
Execute command 'create world'
...
---
Execute command 'destroy world'
...
```

重复使用节点，先用**锚**标记，再用**星号**引用。

```yaml
origin: &anchor 100-100-100
ref: *anchor
```

复杂映射，用**问号**和**空格**表示键，再用**冒号**和**空格**表示值。

```yaml
? - key
: - value
```

**键值对**可以直接跟在**破折号**、**冒号**、**问号**后面。

```yaml
- key: value
  key2: value2
- key3: value3
```

## 参考

<https://yaml.org/spec/1.2/spec.html>
