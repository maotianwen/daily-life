### Echarts

今天看了看Echarts的文档，这真的是国内最优秀的数据可视化工具之一了，除了稍微有点门槛？（感觉设计师上手还是有难度的）


### 单页面应用路由原理

毕设打算做一个SPA，应该是用 Vue + Echarts了，今天了解了单页面应用路由实现原理。
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
