---
title: 编译FTXUI库
published: 2026-04-24
description: '教学如何在Windows中编译FTXUI库'
image: ''
tags: [FTXUI, 编译, Windows, 代码]
category: '技术'
draft: false 
lang: 'zh-CN'
---

最近发现了一个很有意思的 C++ 库——[FTXUI](https://github.com/ArthurSonzogni/FTXUI)，它能让控制台程序变得不再单调，可以做出漂亮的 TUI（终端用户界面）。

折腾了一整天终于成功编译，踩了不少坑，记录一下完整过程，希望能帮到有需要的人。

## 环境准备

在开始之前，请确保你的电脑已经安装了以下工具：

| 工具 | 版本要求 | 下载链接 |
|------|----------|----------|
| Windows 系统 | 任意版本 | - |
| Visual Studio | 2019 / 2022 / 2026 | [官网下载](https://visualstudio.microsoft.com/) |
| Git | 最新版 | [官网下载](https://git-scm.com/install/) |
| 一个正常的脑子 | 必备 | 自备 |

> 💡 我使用的是 VS2026，但 VS2019 或 VS2022 也完全适用，步骤是一样的。

## 拉取项目源码

首先，我们需要从 GitHub 拉取 FTXUI 的源代码。

**⚠️ 重要提示：一定要拉取指定版本的分支，不要拉 main 分支！**

main 分支是开发分支，直接用cmake编译会报些奇奇怪怪的错误。(别问我咋知道的，问就是一天)

```bash
git clone --branch v6.1.9 https://github.com/ArthurSonzogni/FTXUI.git
```

## 创建编译目录

进入项目目录，并创建用于存放编译结果的文件夹：

```bash
cd FTXUI

# 创建 64 位编译目录
mkdir build_x64

# 创建 32 位编译目录（可选）
mkdir build_x32
```

## 打开 VS 开发者命令行

这是关键步骤！**不要用普通的 CMD 或 PowerShell**。

1. 按 `Win + S` 打开 Windows 搜索
2. 搜索 **"x64 Native Tools Command Prompt for VS"**（编译 64 位）
3. 或搜索 **"x86 Native Tools Command Prompt for VS"**（编译 32 位）
4. 右键选择 **"以管理员身份运行"**

## 开始编译

### 编译 64 位版本

在打开的 VS 开发者命令行中执行：

```bash
# 进入 64 位编译目录
cd your_path\FTXUI\build_x64

# 生成项目文件
cmake .. -A x64

# 编译 Debug 版本
cmake --build . --config Debug

# 编译 Release 版本
cmake --build . --config Release
```

### 编译 32 位版本

如果需要 32 位版本，打开 **x86 Native Tools Command Prompt**：

```bash
# 进入 32 位编译目录
cd your_path\FTXUI\build_x32

# 生成项目文件
cmake .. -A Win32

# 编译 Debug 版本
cmake --build . --config Debug

# 编译 Release 版本
cmake --build . --config Release
```

## 获取编译结果

编译完成后，你可以在以下目录找到生成的库文件：

```
FTXUI/
├── build_x64/
│   ├── Debug/          # Debug 版本的库文件
│   └── Release/        # Release 版本的库文件
└── build_x32/
    ├── Debug/          # Debug 版本的库文件
    └── Release/        # Release 版本的库文件
```

## 在你的项目中使用

编译完成后，在你的 C++ 项目中使用 FTXUI：

1. **包含头文件**：将 FTXUI 的 `include` 目录添加到你的项目包含路径
2. **链接库文件**：将编译好的 `.lib` 文件添加到链接器输入中
3. **复制 DLL**：如果使用动态库，将 `.dll` 文件放到可执行文件同级目录

## 常见问题

**Q: 提示 "cmake 不是内部或外部命令"？**
> A: 确保你打开的是 **VS 开发者命令行**，不是普通 CMD。

**Q: 编译报错，提示找不到某些头文件？**
> A: 检查是否拉取的不是 main 分支。

**Q: 编译成功但运行时报错？**
> A: 检查 Debug/Release 版本是否匹配，以及运行时库设置是否一致。

## 总结

FTXUI 是一个非常棒的 TUI 库，虽然编译过程有点折腾，但效果真的很棒的诶。如果你也想让控制台程序变得高大上，强烈推荐试试

有任何问题欢迎在评论区留言交流
