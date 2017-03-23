title: AutoreleasePool
date: 2016-03-21 01:27:31
tags:
---
自动释放池

- 系统
	- 系统默认会在 main.m 文件中创建一个自动释放池
- 手动
	- 例如在for循环中产生大量无法及时释放的临时变量
\`\`\`
for (int i = 0; i \< largeNumber; ++i) {
	NSString *str = @"Hello World";
	str = [str stringByAppendingFormat:@" - %d", i];
	str = [str uppercaseString];
}
\`\`\`
问：以上代码存在什么样的问题？如果循环的次数非常大时，应该如何修改？

	    - 如果不在本次循环结束的时候消除的话节会一直存在直到程序运行过 } 才会被释放占用大量内存
	    - 解决办法:

\`\`\`
-(void)viewDidLoad {
	[super viewDidLoad];
	
	int largeNumber = 1000000;
	for (int i = 0; i < largeNumber; ++i) {
@autoreleasepool {
	NSString *str = @"Hello World";
	str = [str stringByAppendingFormat:@" - %d", i];
	str = [str uppercaseString];
	    }// 循环执行到这里就会回头继续执行循环了
	    //这样在循环内部就已经释放了自动释放池
	}
	// 不加自动释放池的话要执行到这里才会释放对象
}
\`\`\`
自动释放池的 创建 & 销毁

- 每一次 主线程 的 消息循环 开始的时候会创建自动释放池
- 消息循环结束前,会释放自动释放池
- 自动释放池销毁的时候会向其内部所有的对象发送一个 release 消息 释放所有的对象

内存管理-OC

消息循环

- 保证应用程序不退出
- 实现: 一个死循环不断地监视事件 (方法,点击事件等),并执行对应的方法
- [面试会问]app是如何运行的?
![](https://github.com/RyukieSama/Ryukie.GitHub.io/blob/gh-pages/images/79FFBB31-E37D-440D-8870-E3092C958B2C.png?raw=true
)
- 执行流程详细描述:
	- 应用程序开启 -\> 创建一个消息循环 -\> 监视事件 -\> 接收到事件 -\> 执行创建自动释放次并执行相应操作 -\> 将执行中产生的对象丢入刚才创建的那个自动释放池 -\> 消息执行结束 -\> 自动释放池销毁(对其中所有对象发送一个 release消息,销毁对象) -\> 一次消息循环结束 -\> 开始下一次消息循环


