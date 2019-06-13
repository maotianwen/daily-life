### Echarts

今天看了看Echarts的文档，这真的是国内最优秀的数据可视化工具之一了，除了稍微有点门槛？（感觉设计师上手还是有难度的）


### 单页面应用路由原理

毕设打算做一个SPA，用Vue + Echarts吧。今天了解了单页面应用路由实现原理。
主流前端路由系统是通过hash或history实现。

##### Hash路由

url 上的 hash 以 # 开头，原本是为了作为锚点，方便用户在文章导航到相应的位置。因为 hash 值的改变不会引起页面的刷新，聪明的程序员就想到用 hash 值来做单页面应用的路由，并且当 url 的 hash 发生变化的时候，可以触发相应 hashchange 回调函数。

所以我们可以写一个 Router 对象，代码如下：

    class Router {
      constructor() {
        this.routes = {};
        this.currentUrl = '';
      }
        route(path, callback) {
        this.routes[path] = callback || function() {};
      }
      updateView() {
        this.currentUrl = location.hash.slice(1) || '/';
        this.routes[this.currentUrl] && this.routes[this.currentUrl]();
      }
      init() {
        window.addEventListener('load', this.updateView.bind(this), false);
        window.addEventListener('hashchange', this.updateView.bind(this), false);
      }
    }
    
* routes 用来存放不同路由对应的回调函数
* init 用来初始化路由，在 load 事件发生后刷新页面，并且绑定 hashchange 事件，当 hash 值改变时触发对应回调函数


##### History路由

History 路由是基于 HTML5 规范，在 HTML5 规范中提供了 history.pushState || history.replaceState 来进行路由控制。
当你执行 history.pushState({}, null, '/about') 时候，页面 url 会从 http://xxxx/ 跳转到 http://xxxx/about 可以在改变 url 的同时，并不会刷新页面。

先来简单看看 pushState 的用法，参数说明如下：

* state: 存储 JSON 字符串，可以用在 popstate 事件中
* title: 现在大多浏览器忽略这个参数，直接用 null 代替
* url: 任意有效的 URL，用于更新浏览器的地址栏

hash 的改变可以出发 onhashchange 事件，而 history 的改变并不会触发任何事件，这让我们无法直接去监听 history 的改变从而做出相应的改变。

对于一个应用而言，url 的改变(不包括 hash 值得改变)只能由下面三种情况引起：

1. 点击浏览器的前进或后退按钮。
2. 点击a标签。
3. 在JS代码中触发history.push(replace)State函数。

只要对上述三种情况进行拦截，就可以变相监听到 history 的改变而做出调整。针对情况一，HTML5 规范中有相应的 onpopstate 事件，通过它可以监听到前进或者后退按钮的点击，值得注意的是，调用 history.push(replace)State 并不会触发 onpopstate 事件。
    
    class Router{
      constructor(){
        this.routes = {};
        this.currentUrl = '';
      }
      route(path, callback) {
        this.routes[path] = callback || function() {};
      }
      updateView(url) {
        this.currentUrl = url;
        this.routes[this.currentUrl] && this.routes[this.currentUrl]();
      }
      bindLink() {
        const allLink = document.querySelectorAll('a[data-href]');
        for(let i = 0,len = allLink.length;i<len;i++){
          const current = allLink[i];
          current.addEventListener(
            'click',
            e => {
              e.preventDefault();
              const url = current.getAttribute('data-href');
              history.pushState({},null,url);
              this.updateView(url);
            },
            false
           );
        }
      }
      init() {
         this.bindLink();
         window.addEventListener('popstate',e =>{
         this.updateView(window.location.pathname);
         });
         window.addEventListener('load',() => this.updateView('/'),false);
      }
     }

Router 跟之前 Hash 路由很像，不同的地方在于 init 初始化函数，首先需要获取所有特殊的链接标签，然后监听点击事件，并阻止其默认事件，触发 history.pushState 以及更新相应的视图。

另外绑定 popstate 事件，当用户点击前进或者后退的按钮时候，能够及时更新视图，另外当刚进去页面时也要触发一次视图更新。
