[TOC]
# UIWebView 高度适应

## 场景
> 最近项目中遇到Native页面内需要内嵌H5页面的需求
> 内嵌的H5页面用来暂时运营后台动态配置的一些运营内容
> 需要自适应H5内容的高度

## 思路
### 思路一: webViewDidFinishLoad
初遇这个问题时我首先想到的是在`webViewDidFinishLoad` 这个代理方法中通过获取`webView`的`ScrollView`的大小去改变内嵌部分的高度

```swift
- (void)webViewDidFinishLoad:(UIWebView *)webView {
    //TODO: 获取高度并改变容器大小
}

```

但是
这个方法执行的时候页面可能还没有完全展示出来,web页面渲染是需要时间的
所以
在`webViewDidFinishLoad`中去获取到的高度并不准确

### 思路二: KVO
对!
监听`webView`的`scrollView`的contentSize的变化

####添加监听

```swift
[self.hybridWebView.scrollView addObserver:self forKeyPath:@"contentSize" options:NSKeyValueObservingOptionNew context:nil];
```

####触发

```swift
#pragma mark - KVO
- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSKeyValueChangeKey,id> *)change context:(void *)context {
    if ([keyPath isEqualToString:@"contentSize"]) {
       //TODO: 获取高度并改变容器大小
    }
}
```

但是...
现在我用KVO监听出来的高度不对
原因我继续找找...待有结论了再来补充
下面介绍下蠢蠢的方法吧...

### 思路三: 曲线救国
我们现在项目中有是有Native H5混合开发
所有H5页面的请求全部都是调用Native提供的请求方法
所以我想到了在这里做点文章...
但由于请求完成后H5页面的渲染也需要时间,这时直接获取的高度也是不正确的
延时!

```swift
[self performSelector:@selector(改变高度) withObject:self afterDelay:1];
```

我选择在请求成功后延时1s获取高度... [^sample_footnote]
[^sample_footnote]: 这里之前我是设置的0.5秒体验比1s要好,但在有些设备上这0.5秒偶尔会太短高度不准确 所以换成了1s
不得不说效果接近完美...
高度准确,就是有延时,细节体验不是很好
> 这里其实也可以直接提供一个方法让H5页面直接调用去改变高度
> 但是...我就是不想这样...就是不想...不想...



