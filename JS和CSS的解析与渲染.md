>RenderTree渲染与js加载之间，js会被阻塞，是因为渲染用的是GUI线程、js执行用的是js引擎线程就是v8，GUI线程与V8之间是互斥的。又因为浏览器会维持html中css和js的顺序，所以css渲染会阻塞js执行。

### DOM + CSSOM = Render Tree
[id]: https://www.igvita.com/posts/12/doc-render.png
