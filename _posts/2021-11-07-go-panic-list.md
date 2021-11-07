# go panic记录汇总
## waitGroup没有add，直接done，panic无法recover，会导致进程直接退出
```
panic: sync: negative WaitGroup counter
goroutine 731 [running]:
sync.(*WaitGroup).Add(0xc005ed74f0, 0xffffffffffffffff)
/usr/local/go/src/sync/waitgroup.go:74 +0x147
sync.(*WaitGroup).Done(0xc005ed74f0)
/usr/local/go/src/sync/waitgroup.go:99 +0x34
xxxx/xxxServer(0x267e200, 0xc005eea480, 0xc005edb020)
```

## rpc not implements interface 
```
server起不来，出现方法没有实现
ServiceImpl方法没有重写rpc proto编译出来***.ServiceImpl所有的方法
panic: *service.XServiceImpl not implements interface mvp.XServiceImpl
```

## go WaitGroup下面的go routine不执行
在协程里面Add(1)，因协程创建时间开销可能导致`wg.Wait() 提前结束`
```
	rsp := proxy.NewRsp()
	go func() {
		waitGroup.Add(1)
		defer waitGroup.Done()
		proxy.GetKocAssetData(ctx, req, rsp)
	}()
```
https://segmentfault.com/a/1190000017036311 
`b.wg *sync.WaitGroup 不要拷贝传递，要用指针`

## proto冲突，导致panic
has a name conflict over
```
WARNING: proto: file "xxx.proto" has a name conflict over abc.xxx_TIMELINE
A future release will panic on registration conflicts. See:
https://developers.google.com/protocol-buffers/docs/reference/go/faq#namespace-conflict
```
protobuf1.5之后不允许相同package下面出现一模一样的字段,
暂时方案是降低protobuf版本，
``最终方案字段隔离，package下不出现同名字段``

临时方案`go.mod修改`
```
replace (
[github.com/golang/protobuf](http://github.com/golang/protobuf) => [github.com/golang/protobuf](http://github.com/golang/protobuf) v1.4.3
[google.golang.org/protobuf](http://google.golang.org/protobuf) => [google.golang.org/protobuf](http://google.golang.org/protobuf) v1.25.0
)
```