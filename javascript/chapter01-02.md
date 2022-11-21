#### JavaScript实现
> 由三部分组成 
> + 核心(ECMAScript): 提供核心语言功能
> + 文档对象模型(DOM): 提供访问和操作网页内容的方法和接口
> + 浏览器对象模型(BOM): 提供与浏览器交互的方法和接口

#### HTML中使用JavaScript
##### script元素
> script定义了8个属性
> + async：可选。表示应该立即下载脚本，但不应该妨碍页面中其他操作，比如下载其他资源或等待加载其他脚本。只对外部脚本文件有效。
> + charset:可选。表示通过src属性指定的代码的字符集。大多数浏览器会忽略它的值。
> + crossorigin: 可选。配置相关请求的CORS(跨域资源共享)设置。默认不适用CORS。`crossorigin="anonymous"` 配置文件不必设置凭据标识。`crossorigin="use-credentials"` 设置凭据标识，意味着出站请求会包含凭据。
> + defer:可选。表示脚本可以延迟到文档完全被解析和显示之后再执行。只对外部脚本文件有效。
> + integrity:可选。允许对比接收到的资源的签名与指定的加密签名来验证子资源完整性。如果接收到的资源的签名与这个属性指定的签名不匹配，则页面会报错，脚本不会执行。这个属性可以用于确保内容分发网络（CDN）不会提供恶意内容。
> + language: 已废弃。
> + src: 可选。标识包含要执行的代码的外部文件。
> + type: 可选。代替language，标识代码块中脚本语言的内容类型(也称MIME类型)。按照惯例，这个值始终都是`"text/javascript"`,尽管`"text/javascript"`和`"text/ecmascript"`都已经废弃了。JavaScript文件的MIME类型通常是`"application/x-javascript"`,不过给type属性这个值有可能导致脚本被忽略。在非IE中有效地其他值还有`"application/javascript"`和`"application/ecmascript"`。如果这个值是`module`,则代码会被当成ES6模块，而且只有这时候代码中才能出现`import`和`export`关键字。

注意：通过带有src属性的`<script>`元素不应该在`<script>`和`</script>`标签之间再包含其他的额外的JavaScript代码。如果包含了嵌入的代码，**则只会下载并执行外部脚本文件,嵌入的代码会被忽略**。

##### 标签位置
> 把所有JavaScript文件都放在`<head>`里，意味着必须把所有JavaScript代码都下载，解析和解释完成后，才能开始渲染页面(页面在浏览器解析到`<body>`的起始标签时开始渲染)。对于需要很多JavaScript的页面，这会导致页面渲染的明显延迟，在此期间浏览器窗口完全空白。为解决这个问题，通常将所有JavaScript引用放在`<body>`元素中页面内容后面。

##### defer与async
defer：告诉浏览器应该立即下载外部脚本文件，但是执行应该推迟，直到浏览器解析到`</html>`结束标签后才会执行。(JS加载完成，等HTML解析完成后，才执行JS代码)

async: 告诉浏览器不必等脚本下完和执行完后再加载页面，同样不必等到该异步脚本下载和执行后再加载其他脚本。(JS加载完成，执行JS代码，若此时浏览器未解析完成，则停止解析)

异步脚本保证会在页面的`load`事件前执行，但可能会在`DOMContentLoaded`之前或之后。