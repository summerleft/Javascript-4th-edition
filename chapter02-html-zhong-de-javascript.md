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

