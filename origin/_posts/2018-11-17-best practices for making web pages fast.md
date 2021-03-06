---
title: "优化网站性能的35条规则"
image: /assets/img/blog/improveweb.jpg
description: >
  雅虎公司的优化网站性能的35条规则，被开发者称作'雅虎军规'
author: author2
comments: true
---

来源于[Yahoo developer website](https://developer.yahoo.com/performance/rules.html?guccounter=1#menu)的加速网站性能的最佳实践！

### 1. 最小化HTTP请求次数

​	最终用户响应时间的80％用于前端。大部分时间都在下载页面中的所有组件：图像，样式表，脚本，Flash等。减少组件数量反过来减少了呈现页面所需的HTTP请求数量。这是更快页面的关键。

　　减少页面中组件数量的一种方法是简化页面设计。但有没有办法构建内容更丰富的页面，同时还能实现快速响应时间？以下是一些减少HTTP请求数量的技术，同时仍支持丰富的页面设计。

　　组合文件是一种通过将所有脚本组合到单个脚本中来减少HTTP请求数量的方法，并且类似地将所有CSS组合到单个样式表中。当脚本和样式表在不同页面之间变化时，组合文件更具挑战性，但使这部分发布过程可以缩短响应时间。

　　[CSS Sprites](http://alistapart.com/articles/sprites)是减少图像请求数量的首选方法。将背景图像合并为单个图像，并使用CSS`background-image`和`background-position`属性显示所需的图像片段。

​	[图像地图](http://www.w3.org/TR/html401/struct/objects.html#h-13.6)将多个图像组合成单个图像。整体大小大致相同，但减少HTTP请求的数量会加快页面的速度。图像映射仅在图像在页面中是连续的时才起作用，例如导航栏。定义图像映射的坐标可能是乏味且容易出错的。使用图像地图进行导航也无法访问，因此不建议使用。

　　内联图像使用[`data:`URL方案](http://tools.ietf.org/html/rfc2397)将图像数据嵌入实际页面中。这可以增加HTML文档的大小。将内嵌图像组合到（缓存的）样式表中是一种减少HTTP请求并避免增加页面大小的方法。并非所有主流浏览器都支持内嵌图像。

　　减少页面中HTTP请求的数量是可以开始的地方。这是提高首次访问者性能的最重要指南。正如Tenni Theurer的博客文章[浏览器缓存使用 - 暴露！](http://yuiblog.com/blog/2007/01/04/performance-research-part-2/)，您网站的每日访问者中有40-60％使用空缓存。让这些首次访问者快速访问页面是获得更好用户体验的关键。

### 2. 使用内容分发网络（CDN）

​	用户与Web服务器的距离会对响应时间产生影响。在多个地理位置分散的服务器上部署内容将使您的页面从用户的角度加载更快。但是你应该从哪里开始呢？

​	作为实现地理位置分散的内容的第一步，请勿尝试重新设计Web应用程序以在分布式体系结构中工作。根据应用程序的不同，更改体系结构可能包括令人生畏的任务，例如同步会话状态和跨服务器位置复制数据库事务。尝试缩短用户与您的内容之间的距离可能会延迟或永远不会通过此应用程序架构步骤。

​	请记住，最终用户响应时间的80-90％用于下载页面中的所有组件：图像，样式表，脚本，Flash等。这是*性能黄金规则*。而不是从重新设计应用程序架构的艰巨任务开始，最好先分散静态内容。这不仅可以缩短响应时间，而且由于内容交付网络，它更容易实现。

　　内容传送网络（CDN）是分布在多个位置的Web服务器的集合，以更有效地向用户传送内容。选择用于向特定用户传送内容的服务器通常基于网络邻近度的度量。例如，选择具有最少网络跳跃的服务器或具有最快响应时间的服务器。

　　一些大型互联网公司拥有自己的CDN，但使用CDN服务提供商（如[Akamai Technologies](http://www.akamai.com/)，[EdgeCast](http://www.edgecast.com/)或[level3）](http://www.level3.com/index.cfm?pageID=36)具有成本效益。对于初创公司和私人网站来说，CDN服务的成本可能过高，但随着您的目标受众变得越来越大并变得更加全球化，CDN对于实现快速响应时间是必要的。在Yahoo！中，将静态内容从其应用程序Web服务器移动到CDN（如上所述的第三方以及Yahoo自己的[CDN](https://cwiki.apache.org/TS/traffic-server.html)）的属性将最终用户响应时间提高了20％或更多。切换到CDN是一个相对容易的代码更改，将显著提高您的网站的速度。

### 3. 添加Expires或Cache-Control标头

这条规则有两个方面：

- 对于静态组件：通过设置远期未来`Expires`标头实现“永不过期”策略

- 对于动态组件：使用适当的`Cache-Control`标头来帮助浏览器处理条件请求

​	网页设计越来越丰富，这意味着页面中有更多的脚本，样式表，图像和Flash。您的页面的首次访问者可能必须发出多个HTTP请求，但通过使用Expires标头，您可以使这些组件可缓存。这可以避免后续页面查看中不必要的HTTP请求。Expires头文件通常与图像一起使用，但它们应该用于*所有*组件，包括脚本，样式表和Flash组件。

​	浏览器（和代理）使用缓存来减少HTTP请求的数量和大小，从而加快网页加载速度。Web服务器使用HTTP响应中的Expires头来告诉客户端可以缓存组件多长时间。这是一个遥远的未来Expires标题，告诉浏览器这个响应在2010年4月15日之前不会过时。

``` makefile
Expires: Thu, 15 Apr 2010 20:00:00 GMT
```

​       如果您的服务器是Apache，请使用ExpiresDefault指令设置相对于当前日期的到期日期。ExpiresDefault指令的这个示例将Expires日期设置为距请求时间10年。

``` bash
ExpiresDefault "access plus 10 years"
```

​	请记住，如果您使用远期的Expires标头，则必须在组件更改时更改组件的文件名。在Yahoo! 我们经常将此步骤作为构建过程的一部分：版本号嵌入在组件的文件名中，例如，**`yahoo_2.0.6.js`**。

​	使用远期Expires标头仅在用户访问过您的网站后才会影响网页浏览量。当用户第一次访问您的站点并且浏览器的缓存为空时，它对HTTP请求的数量没有影响。因此，此性能改进的影响取决于用户使用已准备好的缓存命中您的页面的频率。（“已准备好的缓存”已包含页面中的所有组件。）我们[在Yahoo!上测量了这一点。](http://yuiblog.com/blog/2007/01/04/performance-research-part-2/)并发现带有固定缓存的页面查看次数为75-85％。通过使用远期的Expires标头，您可以增加浏览器缓存的组件数量，并在后续页面视图中重复使用，而无需通过用户的Internet连接发送单个字节。

### 4. 使用Gzip组件

​	通过前端工程师做出的决策，可以显着减少在网络上传输HTTP请求和响应所需的时间。确实，最终用户的带宽速度，互联网服务提供商，与对等交换点的距离等都超出了开发团队的控制范围。但是还有其他变量会影响响应时间。压缩通过减少HTTP响应的大小来减少响应时间。

从HTTP / 1.1开始，Web客户端表示支持使用HTTP请求中的Accept-Encoding标头进行压缩。

``` makefile
Accept-Encoding: gzip, deflate
```

​	如果Web服务器在请求中看到此标头，它可能会使用客户端列出的方法之一压缩响应。Web服务器通过响应中的Content-Encoding标头向Web客户端通知此情况。

``` makefile
Content-Encoding: gzip
```

​	Gzip是目前最流行，最有效的压缩方法。它由GNU项目开发，并由[RFC 1952](http://www.ietf.org/rfc/rfc1952.txt)标准化。您可能会看到的唯一其他压缩格式是deflate，但效果较差且不太受欢迎。

​	Gzipping通常将响应大小减少约70％。今天大约90％的互联网流量通过声称支持gzip的浏览器传播。如果你使用Apache，配置gzip的模块取决于你的版本：Apache 1.3使用[mod_gzip](http://sourceforge.net/projects/mod-gzip/)而Apache 2.x使用[mod_deflate](http://httpd.apache.org/docs/2.0/mod/mod_deflate.html)。

​	浏览器和代理存在已知问题，这些问题可能导致浏览器期望的内容与压缩内容相关的内容不匹配。幸运的是，随着旧浏览器的使用逐渐减少，这些边缘情况正在逐渐减少。Apache模块通过自动添加适当的Vary响应头来提供帮助。

​	服务器根据文件类型选择gzip的内容，但通常在他们决定压缩的内容方面受到限制。大多数网站都会gzip他们的HTML文档。gzip你的脚本和样式表也是值得的，但很多网站都错过了这个机会。实际上，压缩包括XML和JSON在内的任何文本响应都是值得的。不应对图像和PDF文件进行gzip压缩，因为它们已经过压缩。试图对它们进行gzip不仅浪费CPU，而且可能会增加文件大小。

​	尽可能多地压缩文件类型是减轻页面重量和加速用户体验的简便方法。

### 5. 将CSS样式表放在头部

​	在研究Yahoo!的性能时，我们发现将样式表移动到文档HEAD会使页面*看起来*加载速度更快。这是因为将样式表放在HEAD中允许页面逐步呈现。

​	关注性能的前端工程师希望页面逐步加载; 也就是说，我们希望浏览器尽快显示它拥有的任何内容。这对于具有大量内容的页面和对较慢Internet连接的用户尤其重要。为用户提供视觉反馈（例如进度指示器）的重要性已得到很好的研究和[记录](http://www.useit.com/papers/responsetime.html)。在我们的例子中，HTML页面是进度指示器！当浏览器逐步加载页面时，标题，导航栏，顶部的徽标等都作为等待页面的用户的视觉反馈。这改善了整体用户体验。

​	将样式表放在文档底部附近的问题是它禁止在许多浏览器（包括Internet Explorer）中逐行渲染。这些浏览器会阻止渲染，以避免在样式发生变化时重绘页面元素。用户查看空白页面时卡住了。

​	[HTML规范](http://www.w3.org/TR/html4/struct/links.html#h-12.3)明确指出，样式表是被包括在网页的HEAD：“与A，[LINK]可以仅出现在一个文档的HEAD部分，尽管它可能出现任意次数”。这些替代品，空白的白色屏幕或无风格内容的闪光都是值得冒险的。最佳解决方案是遵循HTML规范并在文档HEAD中加载样式表。

### 6. 将JavaScript脚本放在底部

​	脚本引起的问题是它们阻止了并行下载。[HTTP / 1.1规范](http://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html#sec8.1.4)建议的浏览器下载不超过两种组分在每主机名平行。如果您从多个主机名提供图像，则可以并行执行两次以上的下载。但是，在下载脚本时，即使在不同的主机名上，浏览器也不会启动任何其他下载。

​	在某些情况下，将脚本移到底部并不容易。例如，如果脚本用于`document.write`插入页面内容的一部分，则无法在页面中向下移动。可能还存在范围问题。在许多情况下，有办法解决这些问题。

​	经常出现的另一种建议是使用延迟脚本。该`DEFER`属性表明该脚本不包含document.write，并且是浏览器可以继续呈现的线索。不幸的是，Firefox不支持该`DEFER`属性。在Internet Explorer中，脚本可能会延迟，但不是所需的。如果可以延迟脚本，也可以将其移动到页面底部。这将使您的网页加载速度更快。

### 7. 避免使用CSS中的expressions

​	CSS表达式是一种动态设置CSS属性的强大（且危险）方法。从版本5开始，它们在Internet Explorer中受支持，但从[IE8开始不推荐使用](http://msdn.microsoft.com/en-us/library/ms537634%28VS.85%29.aspx)。例如，可以使用CSS表达式将背景颜色设置为每小时交替：

``` css
background-color: expression((new Date()).getHours()%2 ? "#B8D4FF" : "#F08A00");
```

​	如此处所示，该`expression`方法接受JavaScript表达式。CSS属性设置为评估JavaScript表达式的结果。`expression`其他浏览器会忽略该方法，因此在Internet Explorer中设置属性以在跨浏览器创建一致体验时非常有用。

​	表达式的问题在于它们的评估频率高于大多数人的预期。它们不仅在页面呈现和调整大小时进行评估，而且在页面滚动时甚至在用户将鼠标移动到页面上时进行评估。在CSS表达式中添加计数器可以让我们跟踪CSS表达式的计算时间和频率。在页面上移动鼠标可以轻松生成10,000多个评估。

​	减少CSS表达式求值次数的一种方法是使用一次性表达式，其中第一次计算表达式时，它将style属性设置为显式值，这将替换CSS表达式。如果必须在页面的整个生命周期中动态设置样式属性，则使用事件处理程序而不是CSS表达式是另一种方法。如果必须使用CSS表达式，请记住它们可能会被评估数千次，并可能影响页面的性能。

### 8. 将JavaScript和CSS独立成外部文件

​	其中许多性能规则都涉及外部组件的管理方式。但是，在出现这些考虑因素之前，您应该提出一个更基本的问题：JavaScript和CSS是否应该包含在外部文件中，还是内嵌在页面中？

​	在现实世界中使用外部文件通常会产生更快的页面，因为浏览器会缓存JavaScript和CSS文件。每次请求HTML文档时，都会下载HTML文档中内联的JavaScript和CSS。这减少了所需的HTTP请求数，但增加了HTML文档的大小。另一方面，如果JavaScript和CSS位于浏览器缓存的外部文件中，则HTML文档的大小会减少，而不会增加HTTP请求的数量。

​	因此，关键因素是外部JavaScript和CSS组件相对于请求的HTML文档数量的缓存频率。这个因素虽然难以量化，但可以使用各种指标进行衡量。如果您站点上的用户每个会话有多个页面查看，并且您的许多页面重复使用相同的脚本和样式表，则缓存的外部文件可能会带来更大的潜在好处。

​	许多网站都处于这些指标的中间。对于这些站点，最佳解决方案通常是将JavaScript和CSS部署为外部文件。内联的唯一例外是主页，例如[Yahoo！的首页](http://www.yahoo.com/)和[My Yahoo! ](http://my.yahoo.com/)。每个会话具有很少（可能只有一个）页面视图的主页可能会发现内联JavaScript和CSS会导致更快的最终用户响应时间。

​	对于通常是许多页面视图中的第一个的首页，有一些技术可以利用内联提供的HTTP请求的减少，以及通过使用外部文件实现的缓存优势。其中一种技术是在首页中内联JavaScript和CSS，但在页面加载完成后动态下载外部文件。后续页面将引用应该已存在于浏览器缓存中的外部文件。

### 9. 减少DNS查询

　　域名系统（DNS）将主机名映射到IP地址，就像电话簿将人们的姓名映射到他们的电话号码一样。当您在浏览器中键入www.yahoo.com 时，浏览器联系的DNS解析器将返回该服务器的IP地址。DNS有成本。DNS通常需要20-120毫秒才能查找给定主机名的IP地址。在DNS查找完成之前，浏览器无法从此主机名下载任何内容。

　　缓存DNS查找以获得更好的性能。此缓存可以在由用户的ISP或局域网维护的特殊缓存服务器上进行，但在单个用户的计算机上也存在缓存。DNS信息保留在操作系统的DNS缓存中（Microsoft Windows上的“DNS客户端服务”）。大多数浏览器都有自己的缓存，与操作系统的缓存分开。只要浏览器将DNS记录保存在自己的缓存中，它就不会因操作系统请求记录而烦恼。

　　默认情况下，Internet Explorer会将DNS查找缓存30分钟，具体 `DnsCacheTimeout`取决于注册表设置。Firefox将DNS查找缓存1分钟，由`network.dnsCacheExpiration`配置设置控制。（Fasterfox将此更改为1小时。）

　　当客户端的DNS缓存为空（对于浏览器和操作系统）时，DNS查找的数量等于网页中唯一主机名的数量。这包括页面的URL，图像，脚本文件，样式表，Flash对象等中使用的主机名。减少唯一主机名的数量可减少DNS查找的数量。

　　减少唯一主机名的数量有可能减少页面中发生的并行下载量。避免DNS查找会缩短响应时间，但减少并行下载可能会缩短响应时间。我的准则是将这些组件分成至少两个但不超过四个主机名。这导致在减少DNS查找和允许高度并行下载之间的良好折衷。

### 10. 压缩JavaScript和CSS(包括内联`<script>`和`<style>`)

　　压缩是从代码中删除不必要的字符以减小其大小从而改善加载时间的做法。压缩代码时，将删除所有注释，以及不需要的空格字符（空格，换行符和制表符）。在JavaScript的情况下，这改善了响应时间性能，因为下载文件的大小减小了。用于压缩JavaScript代码的两种流行工具是[JSMin](http://crockford.com/javascript/jsmin)和[YUI Compressor](https://developer.yahoo.com/yui/compressor/)。YUI压缩器也可以压缩CSS。

　　混淆是可以应用于源代码的替代优化。它比压缩更复杂，因此更容易因混淆步骤本身而产生错误。在对美国十大顶级网站的调查中，压缩规模缩小了21％，而混淆缩小了25％。尽管混淆具有更高的大小缩减，但压缩JavaScript的风险较小。

　　除了压缩外部脚本和样式之外，内联`<script>`和`<style>`块也可以并且也应该压缩。即使你gzip你的脚本和样式，压缩它们仍然会减小5％或更多的大小。随着JavaScript和CSS的使用和大小的增加，压缩代码所节省的成本也会增加。

### 11. 避免重定向

　　使用301和302状态代码完成重定向。以下是301响应中HTTP标头的示例：

``` http
HTTP/1.1 301 Moved Permanently
Location: http://example.com/newuri
Content-Type: text/html
```

　　浏览器自动将用户带到该`Location`字段中指定的URL 。重定向所需的所有信息都在标题中。响应的主体通常是空的。尽管有其名称，但实际上不会缓存301或302响应，除非其他标题（例如`Expires`或`Cache-Control`表示它应该是）。元刷新标记和JavaScript是将用户定向到不同URL的其他方法，但如果必须进行重定向，则首选技术是使用标准3xx HTTP状态代码，主要是为了确保后退按钮正常工作。

　　要记住的主要事情是重定向会降低用户体验。在用户和HTML文档之间插入重定向会延迟页面中的所有内容，因为页面中的任何内容都无法呈现，并且在HTML文档到达之前不会开始下载任何组件。

　　最浪费的重定向之一经常发生，Web开发人员通常不了解它。当URL中缺少尾部斜杠（/）时会发生这种情况。例如，转到<http://astrology.yahoo.com/astrology>会产生301响应，其中包含重定向到<http://astrology.yahoo.com/astrology/>（注意添加的尾部斜杠）。如果您使用的是Apache处理程序，则可以使用`Alias `或`mod_rewrite `或 `DirectorySlash`指令在Apache中修复此问题。

　　将旧网站连接到新网站是重定向的另一种常见用途。其他包括连接网站的不同部分并基于特定条件（浏览器类型，用户帐户类型等）指导用户。使用重定向连接两个网站很简单，只需要很少的额外编码。虽然在这些情况下使用重定向会降低开发人员的复杂性，但会降低用户体验。使用重定向的替代方法包括使用`Alias`以及`mod_rewrite`两个代码路径是否托管在同一服务器上。如果域名更改是使用重定向的原因，则另一种方法是创建CNAME（创建从一个域名指向另一个域名的别名的DNS记录）与`Alias`或组合`mod_rewrite`。

### 12. 删除重复的脚本

　　在一个页面中两次包含相同的JavaScript文件会损害性能。这并不像你想象的那么不寻常。对美国十大顶级网站的评论显示，其中两个网站包含重复的脚本。两个主要因素会增加脚本在单个网页中重复的几率：团队规模和脚本数量。当它发生时，重复的脚本会通过创建不必要的HTTP请求和浪费的JavaScript执行来损害性能。

　　不必要的HTTP请求在Internet Explorer中发生，但在Firefox中不发生。在Internet Explorer中，如果外部脚本包含两次且不可缓存，则在页面加载期间会生成两个HTTP请求。即使脚本是可缓存的，当用户重新加载页面时也会发生额外的HTTP请求。

　　除了生成浪费的HTTP请求之外，还浪费了多次评估脚本的时间。无论脚本是否可缓存，这种冗余的JavaScript执行都会在Firefox和Internet Explorer中执行。

　　避免意外包含相同脚本两次的一种方法是在模板系统中实现脚本管理模块。包含脚本的典型方法是在HTML页面中使用SCRIPT标记。

``` html
<script type ="text/javascript" src ="menu_1.0.17.js"></script>
```

　　PHP中的另一种选择是创建一个名为insertScript的函数。

``` php
<? php insertScript("menu.js")?>
```

　　除了防止多次插入相同的脚本之外，此函数还可以处理脚本的其他问题，例如依赖性检查和向脚本文件名添加版本号以支持远期的Expires头。

### 13. 配置实体标记ETag

　　实体标记（ETag）是Web服务器和浏览器用于确定浏览器缓存中的组件是否与源服务器上的组件匹配的机制。（“实体”是另一个词“组件”：图像，脚本，样式表等）。添加ETag以提供验证比上次修改日期更灵活的实体的机制。ETag是唯一标识组件的特定版本的字符串。唯一的格式约束是引用字符串。源服务器使用`ETag`响应头指定组件的ETag 。

``` http
HTTP/1.1 200 OK
Last-Modified: Tue, 12 Dec 2006 03:03:59 GMT
ETag: "10c24bc-4ab-457e1c1f"
Content-Length: 12195
```

　　之后，如果浏览器必须验证组件，它将使用`If-None-Match`标头将ETag传递回原始服务器。如果ETag匹配，则返回304状态代码，从而将响应减少12195字节。

``` http
GET /i/yahoo.gif HTTP/1.1
Host: us.yimg.com
If-Modified-Since: Tue, 12 Dec 2006 03:03:59 GMT
If-None-Match: "10c24bc-4ab-457e1c1f"
HTTP/1.1 304 Not Modified
```

　　ETag的问题在于它们通常使用属性构建，这些属性使它们对托管站点的特定服务器是唯一的。当浏览器从一个服务器获取原始组件并稍后尝试在不同服务器上验证该组件时，ETag将不匹配，这种情况在使用服务器集群处理请求的网站上非常常见。默认情况下，Apache和IIS都在ETag中嵌入数据，这大大降低了在具有多个服务器的网站上成功进行有效性测试的几率。

　　Apache 1.3和2.x的ETag格式是`inode-size-timestamp`。虽然给定文件可以跨多个服务器驻留在同一目录中，并且具有相同的文件大小，权限，时间戳等，但它的inode在服务器与下一个服务器之间是不同的。

　　IIS 5.0和6.0与ETag有类似的问题。IIS上ETag的格式是`Filetimestamp:ChangeNumber`。`ChangeNumber`是用于跟踪IIS配置更改的计数器。后面整个网站的所有IIS服务器`ChangeNumber`是相同这是不太可能的。

　　最终结果是Apache和IIS生成的ETag对于完全相同的组件从一台服务器到另一台服务器不匹配。如果ETag不匹配，则用户不会收到ETag设计的小的，快速的304响应; 相反，他们将获得正常的200响应以及组件的所有数据。如果您只在一台服务器上托管您的网站，这不是问题。但是，如果您有多个服务器托管您的网站，并且您正在使用具有默认ETag配置的Apache或IIS，则您的用户正在变慢页面，您的服务器负载更高，您正在消耗更大的带宽，并且代理服务器没有有效地缓存您的内容。即使您的组件具有远期`Expires`标头，每当用户点击重新加载或刷新时，仍会发出条件GET请求。

　　如果您没有利用ETag提供的灵活验证模型，最好只删除ETag。该`Last-Modified`头验证基于对组件的时间戳。删除ETag会减少响应和后续请求中HTTP标头的大小。此[Microsoft支持文章](http://support.microsoft.com/?id=922733)介绍了如何删除ETag。在Apache中，只需将以下行添加到Apache配置文件即可：

``` gfm
FileETag none
```

### 14. 使Ajax可以缓存

　　Ajax的一个优点是它为用户提供即时反馈，因为它从后端Web服务器异步请求信息。但是，使用Ajax并不能保证用户不会在等待那些异步JavaScript和XML响应返回时大拇指。在许多应用程序中，用户是否保持等待取决于Ajax的使用方式。例如，在基于Web的电子邮件客户端中，用户将一直等待Ajax请求的结果，以查找符合其搜索条件的所有电子邮件。重要的是要记住“异步”并不意味着“瞬时”。

​	为了提高性能，优化这些Ajax响应非常重要。提高Ajax性能的最重要方法是使响应可缓存，如[添加Expires或Cache-controll标头中所述](https://developer.yahoo.com/performance/rules.html?guccounter=1#expires)。其他一些规则也适用于Ajax：

- [Gzip组件](https://developer.yahoo.com/performance/rules.html?guccounter=1#gzip)
- [减少DNS查找](https://developer.yahoo.com/performance/rules.html?guccounter=1#dns_lookups)
- [压缩JavaScript](https://developer.yahoo.com/performance/rules.html?guccounter=1#minify)
- [避免重定向](https://developer.yahoo.com/performance/rules.html?guccounter=1#redirects)
- [配置ETag](https://developer.yahoo.com/performance/rules.html?guccounter=1#etags)

　　我们来看一个例子。Web 2.0电子邮件客户端可能使用Ajax下载用户的通讯簿以进行自动完成。如果用户自上次使用电子邮件Web应用程序以来未修改过她的地址簿，则可以从缓存中读取先前的地址簿响应，如果该Ajax响应可以使用将来的Expires或Cache-Control标头进行缓存。必须通知浏览器何时使用先前缓存的地址簿响应而不是请求新的地址簿响应。这可以通过向地址簿Ajax URL添加时间戳来完成，该时间戳指示用户上次修改其地址簿的时间，例如，`&t=1190241612`。如果自上次下载后地址簿尚未被修改，则时间戳将相同，并且将从浏览器的缓存中读取地址簿，从而消除额外的HTTP往返。如果用户修改了地址簿，则时间戳确保新URL与缓存的响应不匹配，浏览器将请求更新的地址簿条目。

　　即使您的Ajax响应是动态创建的，并且可能仅适用于单个用户，它们仍然可以缓存。这样做可以使您的Web 2.0应用程序更快。

### 15. 尽早清除缓存

　　当用户请求页面时，后端服务器可能需要200到500毫秒才能将HTML页面拼接在一起。在此期间，浏览器在等待数据到达时处于空闲状态。在PHP中，您有函数[flush()](http://php.net/flush)。它允许您将部分准备好的HTML响应发送到浏览器，以便浏览器可以在后端忙于HTML页面的其余部分时开始获取组件。这种好处主要出现在繁忙的后端或轻量级前端。

　　考虑刷新的好地方就在HEAD之后，因为头部的HTML通常更容易生成，并且它允许您包含任何CSS和JavaScript文件，以便浏览器在后端处理时并行地开始获取。

例：

``` html
<!-- css/js -->
</head>
<?php flush();?>
<!-- content -->
<body>
```

　　[雅虎 搜索](http://search.yahoo.com/)开创性研究和真实用户测试，以证明使用此技术的好处。

### 16. 使用GET进行Ajax请求

　　在[雅虎 邮件](http://mail.yahoo.com/)团队发现，在使用时`XMLHttpRequest`，POST在浏览器中实现为两步过程：首先发送标头，然后发送数据。因此最好使用GET，它只需要一个TCP数据包发送（除非你有很多cookie）。IE中的最大URL长度为**2K**，因此如果发送的数据超过**2K**，则可能无法使用GET。

　　一个有趣的副作用是没有实际发布任何数据的POST就像GET一样。根据[HTTP规范](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)，GET用于检索信息，因此当您仅请求数据时，使用GET是有意义的（语义上），而不是将数据发送到服务器端存储。

### 17. 延迟加载组件

　　您可以仔细查看您的页面并问自己：“页面最初渲染必要的是什么？”。其余的内容和组件可以等待。

　　JavaScript是在onload事件之前和之后拆分的理想候选者。例如，如果您有执行拖放和动画的JavaScript代码和库，则可以等待，因为在初始渲染之后拖动页面上的元素。其他寻找后期加载候选者的地方包括隐藏内容（用户操作后显示的内容）和首屏下方的图像。

　　帮助您完成工作的工具：[YUI Image Loader](https://developer.yahoo.com/yui/imageloader/)允许您将图像延迟到折叠下方，[YUI Get实用程序](https://developer.yahoo.com/yui/get/)是一种简单的方法，可以动态地包含JS和CSS。在野外的例子看看[雅虎！](http://www.yahoo.com/)打开Firebug网络面板的[主页](http://www.yahoo.com/)。

　　当性能目标与其他Web开发最佳实践一致时，这是很好的。在这种情况下，渐进增强的想法告诉我们，JavaScript在受支持时可以改善用户体验，但您必须确保即使没有JavaScript也能正常工作。因此，在确保页面正常工作之后，您可以使用一些后期加载的脚本来增强它，这些脚本可以为您提供更多的花俏功能，例如拖放和动画。

### 18. 预加载组件

　　预加载可能看起来与延迟加载相反，但它实际上有不同的目标。通过预加载组件，您可以利用浏览器空闲的时间并请求将来需要的组件（如图像，样式和脚本）。这样，当用户访问下一页时，您可以将大部分组件放在缓存中，并且您的页面将为用户加载更快。

　　实际上有几种类型的预加载：

- **无条件**预加载 - 一旦onload触发，你就可以继续获取一些额外的组件。请访问google.com，了解如何请求加载精灵图片。google.com主页上不需要此精灵图片，但在连续搜索结果页面上需要此精灵图片。
- **条件**预加载 - 基于用户操作，您可以进行有根据的猜测，即用户前进的位置并相应地预加载。在[search.yahoo.com](http://search.yahoo.com/)上，您可以看到在开始输入输入框后如何请求一些额外的组件。
- **预期的**预加载 - 在启动重新设计之前提前预加载。经常在重新设计之后听到：“新网站很酷，但比以前慢”。部分问题可能是用户使用完整缓存访问旧网站，但新网站始终是空缓存体验。您可以通过在启动重新设计之前预加载某些组件来缓解此副作用。您的旧站点可以使用浏览器空闲的时间并请求新站点将使用的图像和脚本

### 19. 减少DOM元素的数量

　　复杂页面意味着要下载更多字节，这也意味着JavaScript中的DOM访问速度更慢。例如，当您想要添加事件处理程序时，如果在页面上循环遍历500或5000个DOM元素，则会有所不同。

　　大量的DOM元素可能是一种通症，即应该通过页面标记来改进，而不必删除内容。您是否使用嵌套表进行布局？您是否只是为了解决布局问题而投入更多`<div>`？也许有一种更好，语义更正确的标记方式。

　　[YUI CSS实用程序](https://developer.yahoo.com/yui/) 提供了很好的布局帮助：grids.css可以帮助您完成整体布局，fonts.css和reset.css可以帮助您去除浏览器的默认格式。这是一个重新开始并考虑标记的机会，例如，`<div>`只有在语义上有意义时才使用s，而不是因为它呈现新行。

　　DOM元素的数量很容易测试，只需键入Firebug的控制台：

``` js
document.getElementsByTagName('*').length;
```

　　那到底有多少DOM元素才算少？检查具有良好标记的其他类似页面。例如[雅虎！主页](http://www.yahoo.com/)是一个非常繁忙的页面，仍然不到700个元素（HTML标记）。

### 20. 跨域拆分组件

　　拆分组件允许您最大化并行下载。由于DNS查询惩罚，请确保您使用的域名不超过2-4个。例如，您可以托管HTML和动态内容，`www.example.org` 并在`static1.example.org`和之间拆分静态组件`static2.example.org`

　　有关更多信息，请查看Tenni Theurer和Patty Chi的“ [在Carpool Lane中最大化并行下载](http://yuiblog.com/blog/2007/04/11/performance-research-part-4/) ”。

### 21. 最小化iframe数量

　　iframe允许将HTML文档插入父文档中。了解iframe如何运作以便有效使用非常重要。

`<iframe>` 优点：


- 帮助缓慢的第三方内容，如徽章和广告
- 安全沙箱
- 并行下载脚本

`<iframe>` 缺点：


- 即使空白也要花钱
- 阻止页面onload
- 非语义

### 22. 尽量不用404s

　　HTTP请求很昂贵，因此发出HTTP请求并获得无用的响应（即404 Not Found）是完全没必要的，并且会在没有任何好处的情况下减慢用户体验。

　　有些网站有帮助404表示 ”你的意思是XXX？”，这对用户体验很有好处，但也浪费了服务器资源（比如数据库等）。特别糟糕的是当外部JavaScript的链接错误并且结果是404时。首先，此下载将阻止并行下载。接下来，浏览器可能会尝试解析404响应主体，就像它是JavaScript代码一样，试图找到可用的东西。

### 23. 减小Cookie大小

　　HTTP cookie的使用有多种原因，例如身份验证和个性化。有关cookie的信息在Web服务器和浏览器之间的HTTP标头中进行交换。保持cookie的大小尽可能低是非常重要的，以尽量减少对用户响应时间的影响。

　　欲了解更多信息，请查看 Tenni Theurer和Patty Chi撰写的[“当Cookie崩溃时”](http://yuiblog.com/blog/2007/03/01/performance-research-part-3/)。这项研究的主要内容：

- 消除不必要的cookie
- 保持cookie大小尽可能低，以尽量减少对用户响应时间的影响
- 请注意在适当的域级别设置cookie，以免其他子域受到影响
- 适当地设置过期日期。较早的Expires日期或者没有更早删除cookie，从而改善了用户响应时间

### 24. 对组建使用cookie-free的域名

　　当浏览器发出静态图像请求并将cookie与请求一起发送时，服务器对这些cookie没有任何用处。所以他们只是没有充分理由创建网络流量。您应该确保使用无cookie请求请求静态组件。创建一个子域并在那里托管所有静态组件。

　　如果您的域名是`www.example.org`，您可以托管您的静态组件`static.example.org`。但是，如果您已经在顶级域上设置了cookie `example.org`而不是`www.example.org`，则所有请求都 `static.example.org`将包含这些cookie。在这种情况下，您可以购买一个全新的域，在那里托管您的静态组件，并保持此域无cookie。雅虎 用途`yimg.com`，YouTube使用`ytimg.com`，亚马逊使用`images-amazon.com`等。

　　在无cookie域上托管静态组件的另一个好处是，某些代理可能拒绝缓存使用cookie请求的组件。在相关说明中，如果您想知道是否应该使用https://example.org或https://www.example.org作为主页，请考虑cookie的影响。省略www会让您别无选择，只能写入cookie `*.example.org`，因此出于性能原因，最好使用www子域并将cookie写入该子域。

### 25. 最小化DOM的访问次数

​	使用JavaScript访问DOM元素的速度很慢，因此为了获得响应更快的页面，您应该：

- 缓存对访问元素的引用
- 更新节点“离线”，然后将它们添加到树中
- 避免使用JavaScript修复布局

　　有关更多信息，请查看 Julien Lecomte 的YUI影院的 [“高性能Ajax应用程序”](http://yuiblog.com/blog/2007/12/20/video-lecomte/)。

### 26. 开发巧妙的事件处理程序

　　有时页面感觉响应性较差，因为过多的事件处理程序附加到DOM树的不同元素，然后执行得太频繁。这就是为什么使用*事件委托*是一个很好的方法。如果a中有10个按钮`div`，则只将一个事件处理程序附加到div包装器，而不是每个按钮一个处理程序。事件冒出来，这样你就可以捕捉事件并找出它来自哪个按钮。

　　您也不需要等待onload事件以便开始使用DOM树执行某些操作。通常，您只需要在树中访问要访问的元素。您不必等待下载所有图像。`DOMContentLoaded`是您可能考虑使用的事件而不是onload，但在所有浏览器中都可用之前，您可以使用具有方法的[YUI事件](https://developer.yahoo.com/yui/event/)实用程序`onAvailable`。

　　有关更多信息，请查看 Julien Lecomte 的YUI影院的 [“高性能Ajax应用程序”](http://yuiblog.com/blog/2007/12/20/video-lecomte/)。

### 27. 优先选择使用`<link>`而非`@import`

　　之前的最佳实践之一声明CSS应位于顶部以允许渐进式渲染。

　　在IE中，`@import`行为与`<link>`在页面底部使用相同，因此最好不要使用它。

### 28 避免使用filters

　　IE专有的`AlphaImageLoader`过滤器旨在解决IE版本<7中的半透明真彩色PNG的问题。该过滤器的问题在于它在下载图像时阻止渲染并冻结浏览器。它还会增加内存消耗，并且每个元素应用，而不是每个图像，因此问题成倍增加。

　　最好的方法是`AlphaImageLoader`完全避免使用优雅降级的PNG8，这在IE中很好。如果你绝对需要`AlphaImageLoader`，使用下划线黑客`_filter`不会惩罚你的IE7 +用户。

### 29. 优化图片

　　设计师完成为您的网页创建图像后，在将这些图像FTP到Web服务器之前，仍然可以尝试一些操作。

- 您可以检查GIF并查看它们是否使用与图像中颜色数对应的调色板大小。使用[imagemagick](http://www.imagemagick.org/)很容易检查 
- `identify -verbose image.gif` 
- 当你在调色板中看到使用4种颜色和256色“槽”的图像时，还有改进的余地。
- 尝试将GIF转换为PNG并查看是否存在保存。通常，有。由于浏览器的支持有限，开发人员经常对使用PNG犹豫不决，但现在已成为过去。唯一真正的问题是真彩色PNG中的alpha透明度，但是GIF也不是真彩色，也不支持变量透明度。所以GIF可以做任何事情，调色板PNG（PNG8）也可以做（动画除外）。这个简单的imagemagick命令导致完全安全的PNG：
- `convert image.gif image.png`
- 我们所说的只是：给PiNG一个机会！”
- 在所有PNG上 运行[pngcrush](http://pmt.sourceforge.net/pngcrush/)（或任何其他PNG优化工具）。例： 
- `pngcrush image.png -rem alla -reduce -brute result.png`
- 在所有JPEG上运行jpegtran。此工具执行无损JPEG操作（如旋转），还可用于优化和删除图像中的注释和其他无用信息（如EXIF信息）。
- `jpegtran -copy none -optimize -perfect src.jpg dest.jpg`

### 30. 优化CSS Sprites　

- 将图像水平排列在精灵中而不是垂直排列通常会导致文件较小。
- 在精灵中组合相似的颜色可以帮助您保持较低的颜色数，理想情况下在256色以下，以适应PNG8。
- “适合移动设备”并且不要在精灵中留下大的间隙。这不会影响文件大小，但需要较少的内存，以便用户代理将图像解压缩为像素图。100x100图像是1万像素，其中1000x1000是100万像素

### 31 不要在HTML中缩放图像

　　不要使用比您需要的更大的图像，因为您可以在HTML中设置宽度和高度。如果您需要，

``` html
<img width="100" height="100" src="mycat.jpg" alt="My Cat" /> 
```

　　那么您的图像（mycat.jpg）应该是100x100px而不是缩小的500x500px图像。

### 32. 减小favicon.ico的大小并缓存

　　favicon.ico是一个保留在服务器根目录中的映像。这是一个必要的邪恶，因为即使你不关心它，浏览器仍然会请求它，所以最好不要回复404 Not Found。此外，由于它位于同一台服务器上，因此每次请求时都会发送cookie。此图像也会干扰下载顺序，例如在IE中，当您在onload中请求额外组件时，将在这些额外组件之前下载favicon。

　　因此，为了减轻拥有favicon.ico的缺点，请确保：

- 它很小，最好不到1K。
- 使用您感觉舒适的设置Expires标头（因为如果您决定更改它，则无法重命名）。您可以在将来几个月安全地设置Expires标头。您可以查看当前favicon.ico的上次修改日期，以做出明智的决定。

[Imagemagick](http://www.imagemagick.org/)可以帮助您创建小的favicons

### 33. 保持组件小于25K

　　此限制与iPhone不会缓存大于25K的组件这一事实有关。请注意，这是**未压缩的**大小。这是缩小很重要的地方，因为单独使用gzip可能还不够。

​	欲了解更多信息，请查看Wayne Shea和Tenni Theurer的[性能研究，第5部分：iPhone可缓存性 - 让它坚持下去](http://yuiblog.com/blog/2008/02/06/iphone-cacheability/) 。

### 34. 将组件拆分到多个文档中

　　将组件打包到多部分文档就像带有附件的电子邮件，它可以帮助您通过一个HTTP请求获取多个组件（请记住：HTTP请求很昂贵）。使用此技术时，首先检查用户代理是否支持它（iPhone不支持）。

### 35. 避免设置空图像的src

　　带有空字符串src属性的图像会出现多个预期。它以两种形式出现：

1. HTML
``` html
<img src ="">
```
2. JavaScript
``` javascript
var img = new Image();
img.src ="";
```
　　两种形式都会产生相同的效果：浏览器向您的服务器发出另一个请求

- Internet Explorer向页面所在的目录发出请求。
- Safari和Chrome会向实际页面提出请求。
- Firefox 3及更早版本的行为与Safari和Chrome相同，但3.5版解决了此问题[[错误444931\]](https://bugzilla.mozilla.org/show_bug.cgi?id=444931)，不再发送请求。
- 遇到空图像时，Opera不执行任何操作。

　　为什么这种行为不好？

1. 通过发送大量意外流量来削弱您的服务器，特别是对于每天获得数百万页面浏览量的页面。
2. 废弃服务器计算周期生成永远不会被查看的页面。
3. 可能会损坏用户数据。如果您通过cookie或其他方式跟踪请求中的状态，则可能会破坏数据。即使图像请求未返回图像，浏览器也会读取并接受所有标头，包括所有cookie。虽然其余的响应被丢弃，但可能已经造成了损害。

　　此行为的根本原因是在浏览器中执行URI解析的方式。此行为在RFC 3986 - 统一资源标识符中定义。当遇到空字符串作为URI时，它被视为相对URI，并根据5.2节中定义的算法进行解析。这个具体的例子是一个空字符串，在5.4节中列出。Firefox，Safari和Chrome都按照规范正确解析空字符串，而Internet Explorer正在解析它，显然符合规范的早期版本RFC 2396 - 统一资源标识符（这已被RFC 3986废弃） 。从技术上讲，浏览器正在做他们应该做的事情来解析相对URI。问题是在这种情况下，

​	HTML5添加了img标记的src属性的描述，以指示浏览器不要在4.8.2节中提出额外的请求：

> src属性必须存在，并且必须包含引用非交互式（可选动画）图像资源的有效URL，该资源既不是分页也不是脚本。如果元素的基URI与文档的地址相同，则src属性的值不能是空字符串。

　　希望浏览器将来不会出现这个问题。不幸的是，`<script src ="">`和`<link href ="">`没有这样的子句。也许还有时间进行调整以确保浏览器不会意外地实现此行为。

　　这条规则的灵感来自雅虎的JavaScript大师Nicolas C. Zakas。有关更多信息，请查看他的文章“ [Empty image src can destroy your site](http://www.nczonline.net/blog/2009/11/30/empty-image-src-can-destroy-your-site/) ”。
