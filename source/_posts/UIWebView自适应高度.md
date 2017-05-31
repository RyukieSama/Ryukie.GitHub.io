title: UIWebView自适应高度
date: 2017-03-24 16:02:31
tags: iOS_UI
categories: 码生
---

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

> 添加监听

```swift
[self.hybridWebView.scrollView addObserver:self forKeyPath:@"contentSize" options:NSKeyValueObservingOptionNew context:nil];
```

> 触发

```swift
#pragma mark - KVO
- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSKeyValueChangeKey,id> *)change context:(void *)context {
    if ([keyPath isEqualToString:@"contentSize"]) {
       //TODO: 获取高度并改变容器大小
    }
}
```

