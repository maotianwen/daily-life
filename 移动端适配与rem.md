### 移动端适配方案（px转换为rem）

rem是CSS相对单位的一种，意思就是根据网页的根元素来设置字体大小，和em的区别是em只根据父元素的字体大小来设置。
现在大部分浏览器默认的根元素font-size是16px。


随便举一个例子
    
    html {
        font-size:16px;
    }
    .ex {
         width:10rem;
         height:10rem;
    }
    
这里的width和height就是160px，而如果我们修改html下的font-size为17px时，它们就会自动变成170px。


### 使用sass的工程

可以在scss中写一个函数:

    @function pxtorem($px){
        $rem: 37.5px;
        @return ($px/$rem) + rem;
    }
    
这样当我们写具体数值的时候就可以调用pxtorem函数了，如 width:pxtorem(90px);
上例中的$rem为37.5px其实是因为iPhone6（比较常见的机型）宽度为375px，所以rem = window.innerWidth / 10;
除以10也没有什么特别的原因，貌似就是不想让html的font-size太大？


### 动态设置html的font-size

1. 利用css的media query。
2.  document.getElementsByTagName('html')[0].style.fontSize = window.innerWidth / 10 + 'px';


