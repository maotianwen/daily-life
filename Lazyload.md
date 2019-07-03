### 定义

可以叫延迟加载，也可以叫惰性加载，懒加载等等。
用户滚动到它们之前，视口外的图像不会加载，在长网页上使用延迟加载将使网页加载更快。
还可以帮助减少服务器压力。

### 实现方式

1.先将图片的src设置为loading.gif,而图片真实路径设置在data-src中，页面滚动的时候计算图片位置与视口位置，图片出现在视口内时src = data-src。
    
    <!DOCTYPE html>
    <html lang = "en">
    <head>
      <meta charset = "UTF-8">
      <title>Lazyload</title>
      <style>
      img{
          display:block;
          margin-bottom:80px;
          height:200px;
      }
      </style>
    </head> 
    <body>
     <img src="images/loading.gif" data-src="images/1.png">
     <img src="images/loading.gif" data-src="images/2.png">
     <img src="images/loading.gif" data-src="images/3.png">
     <script>
        function lazyload() {
          let images = document.getElementsByTagName('img');
          let len = images.length;
          let n = 0;
          return function() {
            let height = window.innerHeight || document.documentElement.clientHeight;
            let scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
            for(let i = n;i<len;i++){
             if(images[i].getBoundingClientRect().top < height + scrollTop){
                if(images[i].getAttribute('src') === 'images/loading.gif'){
                   images[i].src = images[i].getAttribute('data-src');
                }
                n += 1
              }
            }
          }
        }
        let loadImages = lazyload();
        loadImages();
        window.addEventListener('scroll',loadImages,false);
        </script>
        </body>
        </html>
        

变量n用来保存已经加载的图片数量，避免每次都从第一张图片开始遍历。
不过直接将上面的函数绑定在scroll事件上，会高频触发事件。
因此可以对lazyload函数做防抖和节流处理。
