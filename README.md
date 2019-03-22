# h5DevSummary
前端开发技术总结，收集备忘一些常见但可能不易解决的问题。

#### 设置了相同的 height 和 line-height，但移动端可能并没有垂直居中

造成这一问题的原因是移动端对于 line-height 的处理不够统一。

1. table布局
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

1. flex布局
```html
<div class="content">一些文字一些文字</div>
.content {
    display: flex;
    height: 20px;
    justify-content: center;
    align-items: center;
}
```

1. 不设置固定的line-height，通过padding撑高
```html
<div class="content">一些文字一些文字</div>
.content {
    line-height: normal; 
    padding: 6px 0; 
}
```
