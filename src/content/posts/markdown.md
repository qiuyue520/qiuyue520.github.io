---
title: Markdown 示例
published: 2026-02-01
description: 一个简单的 Markdown 博客文章示例。
tags: [Markdown, Blogging, Demo]
category: 示例
draft: true
---

# 一个 h1 标题

段落之间用空行分隔。

第二段。_斜体_，**粗体**，和 `等宽字体`。无序列表
看起来像这样：

- 这个
- 那个
- 另一个

注意 --- 不考虑星号 --- 实际文本
内容从第 4 列开始。

> 引用块是
> 这样写的。
>
> 如果需要，它们可以跨越多个段落。

使用 3 个破折号表示 em-dash。使用 2 个破折号表示范围（例如，"全部在
第 12--14 章"）。三个点 ... 将被转换为省略号。
支持 Unicode。☺

## 一个 h2 标题

这里是一个有序列表：

1. 第一项
2. 第二项
3. 第三项

再次注意，实际文本从第 4 列开始（从左侧
开始 4 个字符）。这是一个代码示例：

    # 让我重申一下...
    for i in 1 .. 10 { do-something(i) }

你可能已经猜到了，缩进 4 个空格。顺便说一句，代替
缩进块，如果你喜欢，可以使用定界块：

```
define foobar() {
    print "Welcome to flavor country!";
}
```

（这使得复制和粘贴更容易）。你可以选择标记
定界块，以便 Pandoc 对其进行语法高亮：

```python
import time
# 快速，数到十！
for i in range(10):
    # （但不要太快）
    time.sleep(0.5)
    print i
```

### 一个 h3 标题

现在是一个嵌套列表：

1. 首先，获取这些配料：

    - 胡萝卜
    - 芹菜
    - 扁豆

2. 煮一些水。

3. 把所有东西倒入锅中，然后按照
    这个算法操作：

        找到木勺
        揭开锅盖
        搅拌
        盖上锅盖
        将木勺危险地平衡在锅柄上
        等待 10 分钟
        回到第一步（或完成后关闭炉灶）

    不要碰到木勺，否则它会掉下来。

再次注意，文本总是在 4 个空格缩进处对齐（包括
上面继续第 3 项的最后一行）。

这里是一个链接到 [网站](http://foo.bar)，到 [本地
文档](local-doc.html)，以及到 [当前文档中的章节标题](#an-h2-header)。这里是一个脚注 [^1]。

[^1]: 脚注文本放在这里。

表格可以看起来像这样：

size material color

---

9 leather brown
10 hemp canvas natural
11 glass transparent

表：鞋子，它们的尺寸，以及它们的制作材料

（上面是表格的标题。）Pandoc 也支持
多行表格：

---

keyword text

---

red Sunsets, apples, and
other red or reddish
things.

green Leaves, grass, frogs
and other things it's
not easy being.

---

水平线如下。

---

这里是一个定义列表：

apples
: 适合制作苹果酱。
oranges
: 柑橘！
tomatoes
: tomatoe 中没有 "e"。

同样，文本缩进 4 个空格。（在每个
术语/定义对之间放置一个空行以使其更加分散。）

这里是一个"行块"：

| 第一行
| 第二行
| 第三行

图片可以这样指定：

[//]: # (![示例图片]&#40;./demo-banner.png " exemplary image"&#41;)

内联数学方程式这样写：$\omega = d\phi / dt$。显示
数学应该有自己的行，并放在双美元符号中：

$$I = \int \rho R^{2} dV$$

$$
\begin{equation*}
\pi
=3.1415926535
 \;8979323846\;2643383279\;5028841971\;6939937510\;5820974944
 \;5923078164\;0628620899\;8628034825\;3421170679\;\ldots
\end{equation*}
$$

注意，你可以反斜杠转义任何标点符号
你希望按字面意思显示，例如：\`foo\`，\*bar\* 等。
