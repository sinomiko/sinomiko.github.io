---
layout: post
title: vscode cpp 插件
categories: [vscode, cpp]
description: vscode-cpp-intelli-sense
keywords: vscode, cpp
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

# vscode配置

## cpp intelli sense

主要插件：

- cmake
- cpp

cpp intelli sense 提供很友好的代码分析补全功能；

- 针对分析部分，如果代码需要精准分析；关闭掉intelli sense engine fallback避免引入无效函数符号
![image](/images/posts/tools/vscode-intelli-sense.png)
关闭效果如下：
![image](/images/posts/tools/vscode-intellisense-code-parse.png)
打开效果
![image](/images/posts/tools/vscode-intelli-sense-code-fallback.png)

- inlay hints可以都打开
![image](/images/posts/tools/vscode-intelli-sense-hints.png)

