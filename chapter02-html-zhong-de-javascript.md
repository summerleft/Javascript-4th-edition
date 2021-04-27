# Chapter02 HTML中的JavaScript

## 2.1 &lt;script&gt;元素

&lt;script&gt;元素有八个属性：

* async：立即开始下载脚本，但不能阻止其他页面动作，如下载资源或等待其他脚本加载，仅对外部脚本文件有效
* charset：使用src属性指定的代码字符集
* crossorigin：配置相关请求的CORS（跨源资源共享）设置，默认不使用。crossorigin="anonymous"配置文件请求不必设置凭据标志。crossorigin="use-credentials"设置凭据标志，意味着出站请求会包含凭据
* defer：脚本可以延迟到文档被完全解析和显示之后再执行，只对外部脚本文件有效
* integrity：允许比对接收到的资源和指定的加密签名以验证子资源完整性（SRI，Subresource Integrity）
* language：废弃
* src：表示包含要执行的代码的外部文件
* type：代替language，表示代码快中脚本语言的内容类型

嵌入行内JavaScript代码：

```javascript
<script>
  function sayHi() {
    console.log("Hi!");
  }
</script>
```

使用行内JavaScript代码时，代码中不能出现&lt;/script&gt;。或者使用转义字符“\”

```javascript
<script>
  function sayScript() {
    console.log("<\/script>");
  }
</script>    
```

包含外部文件中的JavaScript：

```javascript
<script src="example.js"></script>
```

若同时使用src属性和行内JavaScript代码，则只会下载并执行脚本文件，忽略行内代码

不管包含什么代码，只要没有使用defer和asyns属性，浏览器都会按照&lt;script&gt;在页面中出现的顺序依次解释

### 2.1.1 标签位置

现代Web应用程序通常将所有JavaScript引用放在&lt;body&gt;元素中的页面内容后面

```javascript
<!DOCTYPE html>
<html>
  <head>
  <title>Example HTML Page</title>
  </head>
  <body>
  <!-- 这里是页面内容 -->
  <script src="example1.js"></script>
  <script src="example2.js"></script>
  </body>
</html>
```

### 2.1.2 推迟执行脚本

在&lt;script&gt;元素上设置defer属性，相当于告诉浏览器立即下载，但延迟执行

```javascript
<DOCTYPE html>
<html>
  <head>
  <title>Example HTML Page</title>
  <script defer src="example1.js"></script>
  <script defer src="example2.js"></script>
  </head>
  <!-- 这里是页面内容 -->
  </body>
</html>
```

* &lt;script&gt;元素会在浏览器解析到结束的&lt;/html&gt;标签后执行
* 脚本会按照它们出现的顺序执行，但都会在DOMContentLoaded事件之前执行

### 2.1.3 异步执行脚本

async属性和defer类似，不同的是，标记为async的脚本不保证按照它们出现的次序执行

```javascript
<DOCTYPE html>
<html>
  <head>
  <title>Example HTML Page</title>
  <script async src="example1.js"></script>
  <script async src="example2.js"></script>
  </head>
  <body>
  <!-- 这里是页面内容 -->
  </body>
</html>
```

异步脚本保证在页面的load事件前执行，但可能会在DOMContentLoaded之前或之后

### 2.1.4 动态加载脚本

通过向DOM添加script元素可以加载指定脚本

```javascript
let script = document.createElement('script');
script.src = 'gibberish.js';
script.async = false; // 统一动态脚本的加载行为
document.head.appendChild(script);
```

这种方式可能会严重影响性能，为了让预加载器知道这些动态请求文件的存在，可以在文档头部显式声明

```javascript
<link rel="preload" href="gribberish.js">
```

### 2.1.5 XHTML中的变化

### 2.1.6 废弃的语法

## 2.2 行内代码与外部文件

推荐使用外部文件的理由如下：

* 可维护性。开发者可以独立于使用它们的HTML页面来编辑代码
* 缓存。如果两个页面都用到同一个文件，则只需下载一次，加载更快
* 适应未来。不必考虑用XHTML或注释黑科技，包含外部JavaScript文件的语法在HTML和XHTML中一致

## 2.3 文档模式

## 2.4 &lt;noscript&gt;元素

被用于给不支持JavaScript的浏览器提供替代内容，在以下两种情况下，浏览器将显示包含在&lt;noscript&gt;中的内容

* 浏览器不支持脚本
* 浏览器对脚本的支持被关闭

```javascript
<!DOCTYPE html>
<html>
  <head>
  <title>Example HTML Page</title>
  <script defer="defer" src="example1.js"></script>
  <script defer="defer" src="example2.js"></script>
  </head>
  <body>
  <noscript>
    <p>This page requires a JavaScript-enabled browser.</p>
  </noscript>
  </body>
</html>
```





