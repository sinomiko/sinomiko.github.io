---
layout: post
title: cpp coredump 记录汇总
categories: [cpp, cpp coredump]
description: cpp coredump
keywords: cpp coredump, cpp
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

# c++ core 记录汇总

## format core
类似传入参数位置和格式错误
```cpp
    void SetFailed(int error_code, const char* reason_fmt, ...)
        __attribute__ ((__format__ (__printf__, 3, 4)));

    ctx->server_cntl->SetFailed(client_cntl->ErrorCode(), "%s", client_cntl->ErrorText().c_str());
```
测试例子
```cpp
#include <stdio.h>
int main()
{
    int i = 0;
    while (i < 10)
    {
        printf("%d%p%s");
        i++;
    }
    
    return 0;
}
```

