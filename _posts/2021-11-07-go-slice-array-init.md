published: true
layout: post
title: Go 使用技巧
category: go
tags: 
  - go
time: 2021.11.7 11:46:00
description: go 一些初始化技巧
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
# Go 使用技巧

## 设计rpc使用go协程
```
waitGroup := sync.WaitGroup{}
//创建数据
rsp := proxy.NewRsp()
waitGroup.Add(1)
go func() {
   defer waitGroup.Done()
   defer func(){
			// 发生宕机时，获取panic传递的上下文并打印
			if e := recover(); e != nil {
			   var buf [4096]byte
			   n := runtime.Stack(buf[:], false)
			   log.ErrorContext(ctx, errorCode, errorMsg, "err", e, "stack", string(buf[:n]))
			}
	 }()
   proxy.DoRpc(ctx, req, rsp)
}()
```

## 多层map，slice初始化
### 多层map，slice使用make初始化
map：`map1 := make(map[int]int)`
slice: `slice1 := make(map[]int, 0)`
### 多层map，slice立即初始化
方法都是类型后面使用`{}`初始化；
map：`{key:value,key2:value2}`
slice: `{value1,value2}`

### slice搭配map初始化
1 初始化了一个数组，数组里面外层map
```
thingMapping := []map[uint64]map[uint64]bool{
                   { }, { }
                }
```
或者
```
	thingMapping := make([]map[uint64]map[uint64]bool, 2)
```
2.1 里层map再初始化
```
thingMapping := []map[uint64]map[uint64]bool{
                   { }, { }
                }
thingMapping[0] = map[uint64]map[uint64]bool{
    33037:{
        199: true,
        200: true,
    },
}
```
2.2 否则就要使用时候初始化里层map
```
if thingMapping[rontCateID] == nil {
    thingMapping[rontCateID] = make(map[uint64]bool)
}
```

### slice初始化
```
SelectedTags := []*TagInfo{
        &TagInfo{
            TagID:1,
            TagName:"运动",
            Source: 1,
        },
        &TagInfo{
            TagID:2,
            TagName:"休闲",
            Source: 2,
        },
    }
```