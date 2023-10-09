---
layout: post
title: vscode cpp debug
categories: [vscode, cpp]
description: vscode-cpp-debug
keywords: vscode, cpp
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

# vscode cpp debug
使用vscode 直接debug程序代码，不仅利于阅读，也便于调试程序

## cpp debug

主要插件：

- [cpptools-extension-pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools-extension-pack)
- [cpptools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
- [cmake](https://marketplace.visualstudio.com/items?itemName=twxs.cmake)
- [cmake-tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cmake-tools)

可便于调试cpp程序，attach或者run均可；

- 如run ut，并支持单步执行
![run](/images/posts/tools/vs-cpp-ut-debug-run.png)
![debug](/images/posts/tools/vs-cpp-ut-debug-step.png)
- 配置
```json
{
    // 使用 IntelliSense 了解相关属性。 
    // 悬停以查看现有属性的描述。
    // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
    {
        "name": "(gdb) 启动",
        "type": "cppdbg",
        "request": "launch",
        "program": "/root/rocksdb/build/plain_table_db_test",
        "args": [],
        // "args": [
        //     "--flagfile",
        //     "/root/feature_storage_platform/run_env/local/gflags_proxy_liveish.conf",
        //     "--log_dir",
        //     "/workspace/log",
        //     "--host",
        //     "127.0.0.1",
        //     "--port",
        //     "10021"
        // ],
        "stopAtEntry": false,
        "cwd": "${fileDirname}",
        "environment": [],
        "externalConsole": false,
        "MIMode": "gdb",
        "setupCommands": [
            {
                "description": "为 gdb 启用整齐打印",
                "text": "-enable-pretty-printing",
                "ignoreFailures": true
            },
            {
                "description": "将反汇编风格设置为 Intel",
                "text": "-gdb-set disassembly-flavor intel",
                "ignoreFailures": true
            }
        ]
    }
       
    ]
}
```
- 当然也支持传递args参数,如上

```json
        // "args": [
        //     "--flagfile",
        //     "/root/feature_storage_platform/run_env/local/gflags_proxy_liveish.conf",
        //     "--log_dir",
        //     "/workspace/log",
        //     "--host",
        //     "127.0.0.1",
        //     "--port",
        //     "10021"
        // ],
```