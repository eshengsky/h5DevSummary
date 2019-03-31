# h5DevSummary
前端开发技术总结，收集备忘一些常见但可能不易解决的问题。

#### 1. 设置 line-height 后移动端没有垂直居中

造成这一问题的原因是移动端各平台及各浏览器对于 line-height 的处理不够统一，解决方案就是不要去依赖 line-height，改用其它方式居中。注意：如果你要重置 line-height 为默认值，值是 normal 而非 initial.

* flex布局

使用弹性布局来实现水平和垂直居中应该是最优先考虑的方案。

```html
<div class="content">一些文字一些文字</div>
.content {
    display: flex;
    height: 20px;
    justify-content: center;
    align-items: center;
}
```

* 默认line-height，通过padding撑高

如果元素无法被设置为flex或table布局，可以考虑使用padding撑高容器，形成垂直居中效果。尤其是容器中的input输入框，该方案能使得光标高度也正常。

```html
<div class="content">一些文字一些文字</div>
.content {
    font-size: 14px;
    height: 14px;
    line-height: normal; 
    padding: 6px 0; 
}
```

* table布局
```html
<div class="container"><div class="content">一些文字一些文字</div></div>
.container {
    display: table;
}
.content {
    display: table-cell;
    vertical-align: middle;
    font-size: 12px;
}
```

#### 2. 判断history.back()是否能成功返回

有时我们会遇到这种需求：当点击返回按钮时，如果有上一页，则返回上一页；如果没有上一页（用户输入URL或书签等），则返回到首页。但基于安全考虑，浏览器没有提供这样一个api来直接判断能否成功返回。但变通的方法还是有的：能够成功返回，可以断定当前页面一定会被卸载；不能返回到上一页，可以断定当前页面一定不会卸载，这样就可以通过监听 beforeunload 事件来判断返回上一页是否成功。

```js
// 不能成功返回时跳转的URL
var fallbackUrl = 'https://m.lvmama.com';

// 是否存在上一页
var hasHistory = false;

$(window).on('beforeunload', function() {
    hasHistory = true;
});

$('#btnBack').on('click', function() {
    // 尝试返回上一页，如果没有上一页会静默失败，不会抛异常
    window.history.back();

    // hasHistory是异步改变的，所以这里同样要异步判断
    setTimeout(function() {
        // hasHistory为false说明当前页面没有卸载，也就没有能成功返回，那就跳转到fallbackUrl
        if (!hasHistory) {
            window.location.href = fallbackUrl;
        }
    }, 200);
});
```
