# Jquery实现显示和隐藏的4种简单方式

显示和隐藏的效果想必大家都有见到过吧，其实很简单，通过jquery便可轻松实现，下面为大家整理了4种方式，大家可以根据需求自由选择

Html代码：

```
<div class="topicList"> 
<h3><span>学习天地</span></h3> 
<ul> 
<li>1111111111</li> 
<li>2222222222</li> 
<li>333333333</li> 
<li>4444444444</li> 
<li>5555555555</li> 
<li>6666666666</li> 
</ul> 
</div> 

```

Jquery代码：

**第一种实现方式：**

```
1. <script type="text/javascript"> 
$(function(){ 
$(".topicList h3").click(function(){ 
var UL = $(this).next("ul"); 
if(UL.css("display")=="none"){ 
UL.css("display","block"); 
} 
else{ 
UL.css("display","none"); 
} 
}); 
}); 
</script> 
```

**第二种实现方式：**

```
2. <script type="text/javascript"> 
$(function(){ 
$(".topicList h3").toggle(function(){ 
$(this).next("ul").hide(1000); 
},function(){ 
$(this).next("ul").show(1000); 
}); 
}); 
</script> 
```

**第三种实现方式：**

可以使用Jquery提供的show和hide来完成带缓动的显示和隐藏效果，由于两个方法相似，可以直接使用toggle来完成。

```
3. <script type="text/javascript"> 
$(function(){ 
$(".topicList h3").toggle(function(){ 
$(this).next("ul").css("display","none"); 
},function(){ 
$(this).next("ul").css("display","block"); 
}); 
}); 
</script> 
```

**第四种实现方式：**

toggle如果有两个参数，并且都是函数，表示第一次点击执行第一个函数，第二次点击执行第二个函数。

```
4. <script type="text/javascript"> 
$(function(){ 
$(".topicList h3").toggle(topicHandler,topicHandler); 
function topicHandler(){ 
//使用fadeIn、show、slideDown可以完成某个容器的显示 
//使用fadeOut、hide、slideUp可以完成某个容器的隐藏 
//所以可以通过各个的toggle来完成两个之间的轮换 
$(this).next("ul").toggle(1000); 
} 
}); 
</script> 
```



