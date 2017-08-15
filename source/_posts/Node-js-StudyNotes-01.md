title: Nodejs StudyNotes_01
date: 2017-08-15 00:25:00
tags: Nodejs
categories: 码生

---

[TOC]

# Node.js 学习笔记01

> 最近开始学习 `Node.js` 了为了给自己的游戏做服务
> 本来就是个懒人...不喜欢写东西    
> 还是要逼自己一把了

 
## Node.js 事件循环
Node.js是单进程应用程序,但是通过`事件`和`回调`支持并发,所以性能非常高.
每一个API都是异步的,并作为一个独立线程运行,使用`异步函数`调用,并处理并发.
基本上所有的事件机制都是用`观察者模式`实现的.
单线程类似进入一个`while(true)`的死循环中,直到没有事件观察者退出,每个异步事件都会生成一个`事件观察者(eventEmitter)`如果有时间发生就调用该回调函数
> 看到这里感觉类似`iOS`中的`运行循环`

## 事件驱动程序
使用事件驱动模型,当`webServer`接收到请求,就把请求关闭掉进行处理,然后去服务下一个请求.
当请求完成后,放入`loop`中,当达到队列开头,最终返回给用户.
这个模型非常高效且拓展性强,server一直接收请求二不等待任何读写操作.(这也被称为:`非阻塞式IO`或者`事件驱动IO`)
在事件驱动模型中,会生成一个主循环来监听事件,当检测到事件时触发回调函数.

- [ ] loop是个同步队列?应该是遵循`FIFO`的规则即先进先出的把?

![](http://ohfpqyfi7.bkt.clouddn.com/15027293122199.jpg)

## EventEmitter


## Buffer 
JS本身只有字符串数据类型没有二进制数据类型.
但在处理`TCP流`或`文件流`时必须使用二进制数据.因此在Node.js中定义了一个Buffer类,该类来创建一个专门方二进制数据的缓存区.
一个`Buffer`类似于一个`整数数组`,但它对应`V8堆内存`之外的一块`原始内存`.

### 创建方式
#### 方法1: 创建指定字节数的Buffer
```swift
var buf = new Buffer(26);
```

#### 方法2: 通过给定的数组创建Buffer
```swift
var buf = new Buffer([10,20,30]);
```

#### 方法3: 通过一个字符串来创建Buffer
```swift
var buf = new Buffer('HelloWorld');
```

### 写入缓存区
```swift
buf.write(string,offset,length,encoding)
```
返回实际写入的大小,如果控件不足则只会写入部分字符串.

### 读取缓存数据
```swift
buf.toString(encoding,start,end);
```

### 将Buffer转化为JSON对象
```swift
buf.toJson()
```

### 合并Buffer
```swift
var buffA = new Buffer('Hello');
var buffB = new Buffer('World');
var buffC = Buffer.concat([buffA,buffB],6);
console.log(buffC.toString('ascii'));
```
输出结果:

```swift
HelloW
```

### 缓冲区对比
这里对比的应该是`ASCII`

```swift
var buffA = new Buffer('Hello');
var buffB = new Buffer('World');

//缓冲区对比
var result = Buffer.compare(buffA,buffB);
console.log('result=' + result);
if (result == 0) {
    console.log('相同');
} else if (result < 0) {
    console.log('第一个在第二个前面');
} else {
    console.log('第一个在第二个后面');
}
```

输出:

```swift
result=-1
第一个在第二个前面
```

### Copy

### 剪裁 Slice
返回一个新的缓冲区,他和旧的缓冲区指向同一块内存,但是从索引 start 到 end 的位置剪切.

### length

## Stream 流
`Stream` 是一个抽象接口,Node中很多对象都实现了这个接口.例如,对http服务器发起请求的`request`对象就是一个Stream,还有`stdout(标准输出)`.

### 四种流类型
* Readable 可读操作
* Writable 可写操作
* Duplex 可读可写操作
* Transform 操作被写入数据,然后读出结果

### 所有Stream都是EventEmitter的实例 常用事件有
* data 当有数可读时触发
* end 没有更多数据可读时触发
* error 接收和写入的过程发生错误时触发
* finish 所有数据已被写入到底层系统时触发(只有read操作才有的)

### 管道流
管道提供了一个输出流到输入流的机制.
通常用于从一个流中获取数据并将数据传递到另一个流中.

```swift
var fileIn = 'hello.txt';
var fileOut = 'world.txt';
var fs = require('fs');

var readStream = fs.createReadStream(fileIn);
var writeStream = fs.createWriteStream(fileOut);

//管道操作    读取文件写入另一个文件中
readStream.pipe(writeStream);
console.log('end');
```

### 链式流
是通过链接输出流到另一个流并创建多个流操作链的机制.
一般用于管道操作.和链式调用差不多


