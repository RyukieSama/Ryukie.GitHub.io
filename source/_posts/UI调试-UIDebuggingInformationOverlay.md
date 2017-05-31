title: UI调试_UIDebuggingInformationOverlay
date: 2017-05-31 16:00:19
tags: iOS_UI
categories: 码生
---

# UI调试_UIDebuggingInformationOverlay 

>最近看到一篇关于使用Apple私有API进行UI调试的文章(http://ryanipete.com/blog/ios/swift/objective-c/uidebugginginformationoverlay/)
>在这里简单说下怎么使用
>话说这玩意儿本身不是很稳定,有时在调试窗口中就会崩溃...

## 使用方法
直接上代码吧
由于使用的是Apple的私有API所以只能通过下面的方式来调用
写在AppDelegate里即可

```swift
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Override point for customization after application launch.
    [NSClassFromString(@"UIDebuggingInformationOverlay") performSelector:NSSelectorFromString(@"prepareDebuggingOverlay")];
    return YES;
}
```

>注意:
>只能在开发调试阶段使用上架之前一定要干掉不然会被拒掉的哦

## 调起调试窗口
在任意页面使用双指 单击状态栏集即可调起调试窗口
通过按住调试窗口的顶部可以拖动调整调试窗口的位置
也可以通过左上角按钮进行全屏和窗口模式的切换
如图效果:
![](http://ohfpqyfi7.bkt.clouddn.com/14962186443463.png)

## 下面来看看都有哪些功能吧
### View Hierarchy 视图集合
这里会显示当前```Window```下的视图集合,层级关系页十分清晰,点击后面的```i```图标还可查看对应View的详细信息
你还可以在不通```Window```间切换查看
![](http://ohfpqyfi7.bkt.clouddn.com/14962198828930.png)
![](http://ohfpqyfi7.bkt.clouddn.com/14962200654350.png)


### VC Hierarchy 控制器集合
这里的控制器集合和上面的视图集合差不多
提供的信息也挺详细的
![](http://ohfpqyfi7.bkt.clouddn.com/14962202375596.png)
![](http://ohfpqyfi7.bkt.clouddn.com/14962202461191.png)
![](http://ohfpqyfi7.bkt.clouddn.com/14962202549313.png)

### Ivar Explorer 变量
这里我们可以看到```UIApplication```的所有变量
感觉实际用处不是很大,变量过多也没有筛选功能,而且...有时滑滑还会Crash掉😂
![](http://ohfpqyfi7.bkt.clouddn.com/14962206787017.png)

### Measure 测量
挺实用的功能
有几种模式可以选择
使用方法: 
手指长按屏幕移动,调试窗口中有个红色小方块对应的就是当前你按压的点

#### 水平测量 
* 不开启```ViewMode```
 
 以你按压的点的位置水平方向,最近的两个可见的轮廓边缘最近距离,无视控件
 ![](http://ohfpqyfi7.bkt.clouddn.com/14962215907592.png)
 通过下面这张图可以明显的感觉到这个测量是根据轮廓来的...居然测了我老婆头发边缘到左边的距离
 ![](http://ohfpqyfi7.bkt.clouddn.com/14962222065291.png)

* 开启```ViewMode```
开启```ViewMode```后测量的就是按压所在控件的尺寸了
![](http://ohfpqyfi7.bkt.clouddn.com/14962228241136.png)

#### 垂直测量 
具体和水平测量差不多我就不多赘述了
* 不开启```ViewMode```
 ![](http://ohfpqyfi7.bkt.clouddn.com/14962216076414.png)
* 开启```ViewMode```
![](http://ohfpqyfi7.bkt.clouddn.com/14962227921357.png)

### Spec Compare 效果对比
这个功能在对比UI效果还原度上真乃神器也~
首先把UI妹纸给你的UI图发到手机上
在需要对比的页面上双击状态栏唤起调试工具
点击 ```Spec Compare``` 进入下面的页面
点击右上 ```Add``` 从手机相册导入UI图
![](http://ohfpqyfi7.bkt.clouddn.com/14962193092278.png)
点击导入的UI图即可进入对比模式
调试工具会将导入的UI图覆盖在当前页面上
通过单指上下滑动即可调整透明度,从而进行UI还原度的对比,效果如下
![](http://ohfpqyfi7.bkt.clouddn.com/14962193475733.png)
双击屏幕即可退出对比模式

### System Color Audit 
还没发现有啥用...点进去啥都木有来着
待发掘



