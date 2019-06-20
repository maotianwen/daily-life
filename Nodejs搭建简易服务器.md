## Node.js搭建服务器
    let http = require('http');

    let hostName = '127.0.0.1';

    let port = 8080;

    let server = http.createServer(function (request, response) {

        // 发送 HTTP 头部 
        // HTTP 状态值: 200 : OK
        // 内容类型: text/plain
        response.setHeader('Content-Type', 'text/plain');


        // 发送响应数据 "Hello World"
        response.end('Hello World\n');
    });

    server.listen(port, hostName, function () {
    // 终端打印如下信息
    console.log(`Server running at http://${hostName}:${port}/`);

    });

在这之后想用一个小demo来向服务器发起请求，就会因为跨域问题而报错。
于是在服务器的响应头文件里改为如下代码：
    
    response.setHeader('Content-Type','text/plain');
        response.setHeader('Access-Control-Allow-Origin','*')
        response.setHeader('Access-Control-Allow-Headers', 'Origin,X-Requested-With,Content-Type,Accept')
        response.end('Hello World');

至少没报错了。

