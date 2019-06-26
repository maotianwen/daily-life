### ES6模块与CommonJS模块的差异

它们有两个重大差异。

1. CommonJS模块输出的是一个值的拷贝，ES6模块输出的是值的引用。
2. CommonJS模块是运行时加载，ES6模块是编译时输出接口。

第二个差异是因为CommonJS加载的是一个对象（即module.exports属性），该对象只有在脚本运行完才会生成。而ES6模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。

ES6输入的模块变量，只是一个“符号连接”，所以这个变量是只读的，对它进行重新赋值会报错。

### 为什么是node_modules和package.json?
 

###### Why not node_packages or module.json ?

>The package.json file defines the package.  

The node_modules folder is the place Node.js looks for modules.  

For example, if you create a file at **node_modules/foo.js** and then had a program that did **var f = require('foo.js')**, it    
would load the module. However, **foo.js** is not a "package" in this case because it does not have a package.json.  





没有package.json文件的不能叫package,它可以是module。node_modules是Node.js用来寻找模块的。





>Alternatively, if you create a package which does not have an index.js or a "main" field in the package.json file, then it is  
not a module. Even if it's installed in node_modules, it can't be an argument to require().



或者，如果你创建了一个package，而这个package里没有index.js。或者这个package的package.json中没有"main"这一项，那它也不能称作一个模块，即使它被安装在node_modules中，它也不能作为require()函数的参数。
