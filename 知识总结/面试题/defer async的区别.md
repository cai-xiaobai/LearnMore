## defer async的区别

- async : 并行加载js，`下载完毕立即解释执行代码`，不会按照页面上的script顺序执行。
- defer : 并行下载js，在页面解析完毕之后，会按照页面上的script标签的顺序执行,同时会在document的DOMContentLoaded之前执行。

