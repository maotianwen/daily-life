>RenderTree渲染与js加载之间，js会被阻塞，是因为渲染用的是GUI线程、js执行用的是js引擎线程就是v8，GUI线程与V8之间是互斥的。又因为浏览器会维持html中css和js的顺序，所以css渲染会阻塞js执行。

### DOM + CSSOM = Render Tree
![picture](/img/doc-render.png)

* The document is marked as “interactive” when the user agent stops parsing the document. Meaning, the DOM tree is ready.
   <br>当用户代理停止解析文档时，文档被标记为“interactive”。也就是说，DOM树已经准备好了。
  
* The user agent fires the DOMContentLoaded (DCL) event once any scripts marked with “defer have been executed, and there are no stylesheets that are blocking scripts. Meaning, the CSSOM is ready.
  <br> 一旦执行了标有“deferred”的脚本，并且没有样式表阻塞脚本，用户代理就会触发DOMContentLoaded (DCL)事件。也就是说，CSSOM已经准备好了。

* If you add a script and tag it with “defer”, then you unblock the construction of the DOM: the document interactive state does not have to wait for execution of JavaScript. However, note that this same script will be executed before DCL is fired. Further, recall that JavaScript may query CSSOM, which means that the DCL event may be held until the CSSOM is ready, at which point the script will be executed. In short: we’ve unblocked the “document interactive” state, but we’re still potentially blocking DCL.
<br>如果添加了一个脚本并将其标记为“defer”，那么您就解除了DOM构造的阻塞:文档交互状态不必等待JavaScript的执行。但是，请注意，在触发DCL之前将执行相同的脚本。此外，请记住JavaScript可能会查询CSSOM，这意味着DCL事件可能会一直保持到CSSOM就绪，然后脚本才会执行。简而言之:我们已经解除了“文档交互”状态的阻塞，但仍然可能阻塞DCL。

* If you add a script and tag it with “async”, then you inherit similar behavior as above, but with one distinction: DCL does not have to wait for execution of async scripts!
<br>如果添加一个脚本并添加“async”，那么将继承与上面类似的行为，但是有一个区别:DCL不必等待async脚本的执行!
