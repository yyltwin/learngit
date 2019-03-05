## 学习目标

[TOC]

### 1-【了解】综合案例：选项卡

```
分析：
1. 布局上，html中分上下两部分，上面是标题栏，提供给用户点击/鼠标悬放；下面是内容显示栏，里面由多个内容窗口，与标题的数量对应。
2. 批量获取标题栏中的标题，组成一个数组，并给每一个标题，批量绑定事件。
3. 每次用户点击不同的标题，根据标题的下标，控制显示对应序号的内容窗口。
   内容窗口的显示，可以通过left定位来实现，也可以通过display:none;来实现。

```



```html
<style>
    .tab_con{
        width:500px;
        height:350px;
        margin:50px auto 0;
    }
    .tab_btns{
        height:50px;
    }
    .tab_btns input{
        width:100px;
        height:50px;
        background:#ddd;
        border:0px;
        outline:none;
    }

    .tab_btns .active{
        background:gold;
    }

    .tab_cons{
        height:300px;
        background:gold;
    }

    .tab_cons div{
        height:300px;
        line-height:300px;
        text-align:center;
        display:none;
        font-size:30px;
    }

    .tab_cons .current{
        display:block;
    }
</style>
<script src="js/jquery-1.12.4.min.js"></script>
<script>
    $(function () {
        //1.获取到所有按钮的jquery元素
        var $btn=$('.tab_btns input');
        // 2.获取到所有按钮对应的内容的jquery元素
        var $div=$('.tab_cons div');
        //3 .给按钮添加点击事件
        $btn.click(function () {
            //给当前的button添加样式
            $(this).addClass('active').siblings().removeClass('active')
            //获取点击以后的下标
            var index=$(this).index()
            //根据下标寻找对应的div
            $div.eq(index).addClass('current').siblings().removeClass('current')
        });
    });
</script>
<body>
    <div class="tab_con">
        <div class="tab_btns">
            <input type="button" value="按钮一" class="active">
            <input type="button" value="按钮二">
            <input type="button" value="按钮三">
        </div>
        <div class="tab_cons">
            <div class="current">按钮一对应的内容</div>
            <div>按钮二对应的内容</div>
            <div>按钮三对应的内容</div>
        </div>
    </div>
</body>
```

代码效果：

![1544042361063.gif](D:/%E5%A4%AA%E7%99%BD%E9%87%91%E6%98%9F/%E9%AA%91%E5%A3%AB%E8%AE%A1%E5%88%92%E5%85%A8%E6%A0%88%E4%B8%89%E6%9C%9F/day38%20%E8%A7%86%E9%A2%91%E4%BB%A5%E5%8F%8A%E7%AC%94%E8%AE%B0/assets/1544042361063.gif)

总结：

```
1. 选项卡的核心实现代码是，通过$(this).index() 来获取当前用户操作的元素的下标。
   然后通过操作对应下标的内容框的显示，达到选项卡的效果。

2. 选项卡的高亮效果实现，是通过$(this)操作当前元素的样式，然后通过siblings()去除其他兄弟元素的样式。

```



### 2-【掌握】jQuery的动画效果

jQuery提供了一个animate方法，给开发者控制元素的css样式，实现动画效果。
animate参数：
​    参数一：要改变的样式属性值，写成对象的形式
​    参数二：动画持续的时间，单位为毫秒，一般不写单位
​    参数三：动画曲线，默认为‘swing’，缓冲运动，还可以设置为‘linear’，匀速运动
​    参数四：动画回调函数，动画完成后执行的匿名函数

```javascript	
    $("div").animate({
        "width": "1000px",
        "height": "300px",
        "font-size": "30px"
    }, 2000, "linear",function(){
        alert("动画结束了");
    })
```



### 01-【掌握】jquery预设的特殊动画效果

jQuery中除了有.animate()方法实现动画，还自带了一些特殊的动画效果：

```javascript
    对角线动画：
        $("div").show(400);   // 显示
        $("div").hide(400);   // 隐藏
        $("div").toggle(400); // 在隐藏和显示之间进行切换
    滑动动画：
        $("div").slideDown(400);  // 向下滑动
        $("div").slideUp(400);    // 向上滑动
        $("div").slideToggle(400); // 上下滑动之间进行切换
    透明度动画：
        $("div").fadeIn(400);     // 淡入
        $("div").fadeOut(400);    // 淡出
        $("div").fadeToggle(400); // 淡入和淡出之间进行切换
        $("div").fadeTo(400,0.5); // 设置元素变化指定透明度
```

这些动画函数的参数使用类似于animate函数，只是animate函数第一个参数是对象，需要设置动画完成的效果。

这些预设动画函数不需要设置第一个效果的参数：

```
show(动画完成的时间[单位:ms],'动画完成时的过渡效果',动画完成以后调用的匿名函数);
```



##### 01.1-jquery的预设动画排队机制

在频繁触发动画效果的时候，jQ会自动记录触发的总次数，直到把所有动画执行完毕为止。 
​    可以通过加.stop()方法来解决频繁触发动画的问题：   （在动画函数前面加.stop()）

```javascript
例如：
    $(this).children("ul").stop().show(500);
```





### 02-【了解】-综合案例：制作层级菜单

```
分析：
1. 层级菜单[不管多少层级]，一般使用无序列表嵌套来完成的，每一个子列表里面都会是标题以及子菜单一起存放
2. 先获取一级菜单，给多个一级菜单，批量绑定事件[点击事件]，每次点击一级菜单，就会显示当前菜单的后面一个内容标签.
```



```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>层级菜单</title>
	<style type="text/css">
		body{
			font-family:'Microsoft Yahei';
		}
		body,ul{
			margin:0px;
			padding:0px;
		}
		ul{list-style:none;}
		.menu{
			width:200px;
			margin:20px auto 0;
		}
		.menu .level1,.menu li ul a{
			display:block;
			width:200px;
			height:30px;
			line-height:30px;
			text-decoration:none;
			background-color:#3366cc;
			color:#fff;
			font-size:16px;
			text-indent:10px;			
		}
		.menu .level1{
			border-bottom:1px solid #afc6f6;
			
		}
		.menu li ul a{
			font-size:14px;
			text-indent:20px;
			background-color:#7aa1ef;
					
		}
		.menu li ul li{
			border-bottom:1px solid #afc6f6;
		}
		.menu li ul{
			display:none;
		}
		.menu li ul.current{
			display:block;
		}
		.menu li ul li a:hover{
			background-color:#f6b544;
		}
	</style>
	<script src="js/jquery-1.12.4.min.js"></script>
	<script>
	$(function(){
		// 单击一级菜单，显示隐藏 滑动 二级菜单
		$('.level1').click(function(){
			$(this).next().slideDown().parent().siblings().children('ul').slideUp()
		})
	})
	</script>
</head>
<body>
	<ul class="menu">
		<li>
			<a href="#" class="level1">手机</a>
			<ul class="current">
				<li><a href="#">iPhone X 256G</a></li>
				<li><a href="#">红米4A 全网通</a></li>
				<li><a href="#">HUAWEI Mate10</a></li>
				<li><a href="#">vivo X20A 4GB</a></li>
			</ul>
		</li>
		<li>
			<a href="#" class="level1">笔记本</a>
			<ul>
				<li><a href="#">MacBook Pro</a></li>
				<li><a href="#">ThinkPad</a></li>
				<li><a href="#">外星人(Alienware)</a></li>
				<li><a href="#">惠普(HP)薄锐ENVY</a></li>
			</ul>
		</li>
		<li>
			<a href="#" class="level1">电视</a>
			<ul>
				<li><a href="#">海信(hisense)</a></li>
				<li><a href="#">长虹(CHANGHONG)</a></li>
				<li><a href="#">TCL彩电L65E5800A</a></li>				
			</ul>
		</li>
		<li>
			<a href="#" class="level1">鞋子</a>
			<ul>
				<li><a href="#">新百伦</a></li>
				<li><a href="#">adidas</a></li>
				<li><a href="#">特步</a></li>
				<li><a href="#">安踏</a></li>
			</ul>
		</li>
		<li>
			<a href="#" class="level1">玩具</a>
			<ul>
				<li><a href="#">乐高</a></li>
				<li><a href="#">费雪</a></li>
				<li><a href="#">铭塔</a></li>
				<li><a href="#">贝恩斯</a></li>
			</ul>
		</li>
		
	</ul>
</body>
</html>
```



总结：

```
1. 层级菜单的实现，使用了动画来完成。
   思路：用户当前操作的1级菜单显示子菜单，其他的所有1级菜单全部隐藏掉。

2. 事件中的$(this) 只带的是当前用户操作的元素，不是选择器，不能代表多个元素。
   $(".level1").next();   页面中所有的class=level1的下一个ul[ 代表多个ul ]

3. jQuery的动画如果频繁触发，会一直持续的完成，如果希望不出现延时的效果，则需要使用.stop();
```





### 03-【了解】综合案例：实现无缝滚动动画效果

```javascript
效果分析：
1. 默认让图片列表整体向左不断移动。
2. 鼠标放上去会暂停移动，鼠标移开又会继续向左移动。
3. 左右两边的按钮，可以控制图片列表的左右移动。
无缝滚动的原理分析：
	 列表的成员赋值一份或或者多份，当第一个列表跑出窗口，然后立刻重置left值。
```

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
    .scroll{
        width: 1100px;
        margin: auto;
        /*border: 1px solid #000;*/
    }
    ul{
        list-style: none;
        margin:0;
        padding: 0;
    }
    .scroll .box{
        width: 900px;
        /*border: 1px solid #000;*/
        overflow: hidden;
        margin: auto;
    }
    .scroll .box li{
        /*width: 200px;*/
        /*text-align: center;*/
        float: left;
    }
    .scroll .image-list{
        width: 3000px;
        overflow: hidden;
    }
    .scroll{
        position: relative;
    }
    .scroll .left{
        position: absolute;
        left:0;
        top:0;
        bottom:0;
        margin:auto;
        display: block;
        height: 40px;
        width: 40px;
        font-size: 36px;
        line-height: 40px;
        color: #fff;
        background: #000;
        border-radius: 100px;
        text-align: center;
    }
    .scroll .right{
        position: absolute;
        right: 0;
        top:0;
        bottom:0;
        margin:auto;
        display: block;
        height: 40px;
        width: 40px;
        font-size: 36px;
        line-height: 40px;
        color: #fff;
        background: #000;
        border-radius: 100px;
        text-align: center;
    }
    </style>
    <script src="js/jquery-1.12.4.js"></script>
    <script>
    $(function(){
        // jQuery没有定时器，所以我们还是用js
        // 让图片列表不断向左移动[ 本质：margin-left不断减值 ]
        // 初始化margin-left的值
        var marginLeft = 0;
        var timer;      // 定时器的值
        var step = -1;  // 初始化图片列表的移动速度，负数表示向左移动，正数表示向右移动
        function move(){
            timer = setInterval(function(){
                // 使用jQuery减去margin-left的值
                marginLeft += step;
                if(marginLeft<-900){
                    //　当图片滚动到-900[5张图片的距离，则表示一个图片列表滚动完成了]
                    marginLeft = 0;
                }
                if(marginLeft>0){
                    marginLeft=-900;
                }
                $(".image-list").css("margin-left",marginLeft+"px");
            },5);
        }

        // 页面一开始就执行滚动效果
        move();

        // 控制暂停
        $(".image-list").hover(function(){
            // 鼠标移入，暂停移动效果
            clearInterval(timer);
        },function(){
            // 鼠标移出，开始移动效果
            move();
        });

        // 左右按钮控制图片列表的显示方向
        // 可以把图片列表移动的速度设置称一个变量，
        // 左移动 -1
        // 右移动 1
        $(".left").on("click",function(){
            step = -1;
        });
        $(".right").on("click",function(){
            step = 1;
        });
        // 速度是可以用来控制方向.
    });
    </script>
</head>
<body>
<div class="scroll">
    <!--左右按钮-->
    <span class="left">&lt;</span>
    <span class="right">&gt;</span>
    <div class="box"><!-- 在ul外层包裹一个div的作用就是使用overflow:hidden，隐藏ul超出范围的li -->
        <ul class="image-list">
            <li><img src="images/goods001.jpg" alt=""></li>
            <li><img src="images/goods002.jpg" alt=""></li>
            <li><img src="images/goods003.jpg" alt=""></li>
            <li><img src="images/goods004.jpg" alt=""></li>
            <li><img src="images/goods005.jpg" alt=""></li>
            <li><img src="images/goods001.jpg" alt=""></li>
            <li><img src="images/goods002.jpg" alt=""></li>
            <li><img src="images/goods003.jpg" alt=""></li>
            <li><img src="images/goods004.jpg" alt=""></li>
            <li><img src="images/goods005.jpg" alt=""></li>
        </ul>
    </div>
</div>
</body>
</html>
```

总结：

```
1. 滚动图片的总宽度要等于外框的总体宽度。
2. 可以使用速度来控制元素滚动的方向.
   step = -1 ;  // 向左移动
   step = 1;    // 向右移动
3. 判断滚动图片列表是否超出范围，需要通过marginLeft和图片总宽度来判断，进行重置marginLeft;
    if(marginLeft<-900){
        //　当图片滚动到-900[5张图片的距离，则表示一个图片列表滚动完成了]
        marginLeft = 0;
    }
    if(marginLeft>0){
    		marginLeft=-900;
    }
    
    // 上面的判断条件不能是=等号。
```





### 04- 【了解】事件冒泡

在一个HTML元素上触发某类事件（比如单击onclick事件），如果此对象定义了此事件的处理程序，那么此事件就会调用这个处理程序，如果没有定义此事件处理程序或者事件返回true，那么这个事件会向这个元素的父级元素传播事件，从里到外，直至它的所有父级元素所有同名事件都将被激活，这种事件的传播，我们称之为"事件冒泡"。

##### 04.1- 事件冒泡的影响

好处：我们可以通过事件冒泡可以直接在父级元素上面绑定事件来同时处理多个同样的子元素操作。

坏处：在不需要触发父元素的同名事件时，可以自动触发。



##### 04.2- 阻止事件冒泡

阻止事件冒泡，可以通过 event.stopPropagation() 来阻止或者return false;来解决。



##### 04.3- 阻止元素的事件行为

阻止表单的提交行为

```javascript
$('#form1').on("submit",function(event){
    event.preventDefault(); // 方法2: 通过 事件对象.preventDefault(); 阻止表单提交
    return false;           // 方式1，使用return false; 阻止表单提交
})
```

阻止链接的跳转行为

```
$("#a1").on("click",function(){
   return false;
});
```



总结：

```
1. 事件冒泡，就是触发了子元素的事件以后，会自动把父类元素的同名事件也触发到。

1. 阻止表单提交，链接跳转和事件冒泡都可以使用return false;
```



##### 04.3- 案例：页面弹框（点击弹框外弹框关闭）

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
    .login{
        width: 400px;
        height: 280px;
        background: #fff;
        border: 1px solid #aaa;
        position: absolute;
        top:0;
        left:0;
        right:0;
        bottom:0;
        margin:auto;
        display: none;
    }
    .opacity{
        display: none;
        background: #000;
        position: absolute;
        top:0;
        left:0;
        right:0;
        bottom:0;
        margin:auto;
        opacity: 0.3;
    }
    </style>
    <script src="js/jquery-1.12.4.js"></script>
    <script>
    $(function(){
       // 点击登陆显示登陆窗口
        $(".btn").on("click",function(e){ // 事件的匿名函数被调用的时候，系统会在匿名函数这里传递一个参数，就是事件对象
            $(".opacity").show();
            $(".login").show();
             return false; // 方法1，使用return false可以解决事件的冒泡
            // e.stopPropagation(); // 方法2:使用事件对象的stopPropagation()来组织冒泡
        });

        $("body").on("click",function(){
//            alert("body标签被点了");
            $(".opacity").hide();
            $(".login").hide();
        });

        $(".login").on("click",function(){
            return false; // 阻止事件冒泡
        })
    });
    </script>
</head>
<body>
    <span class="btn">登陆</span>
    <div class="opacity"></div><!-- 遮罩层 -->
    <div class="login">
        <div class="content">登陆窗口</div>
    </div>
</body>
</html>
```



### 05-【掌握】事件委托

事件委托就是利用冒泡的原理，把事件加到父级上，通过判断事件来源的子集，执行相应的操作，事件委托首先可以极大减少事件绑定次数，提高性能；其次可以让新加入的子元素也可以拥有相同的操作。

```html
<script>
$(function(){
    // 旧版本delegate，新版本使用on
    $('#list').on('click','li', function() {
        $(this).css({background:'red'});
    });
})
</script>
...
<ul id="list">
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
</ul>
```



总结：

```
1. 利用了事件冒泡的原理，把事件绑定在父级元素的身上，然后在on的第二个参数上，锁定事件源，进行统一的操作。这种操作就是事件委托。
   $("父级元素的选择器").on("事件类型","事件发生源的css选择器",function(){
        // 针对事件发生的子元素的统一操作
   });
```





### 06-【了解】DOM操作

注意：DOM操作是在复杂的，较大的特效里面，是不能使用的。

dom，是document object model的缩写，翻译：文档对象模型，是指使用js改变html文档的标签结构。

一般有两种操作情况：

1. 将新创建的标签插入到现有的标签中。
2. 移动现有标签的位置。

##### 06.1- 创建新标签

```
var $div = $('<div>'); //创建一个空的div
var $div2 = $('<div>这是一个div元素</div>');
```



##### 06.2-移动或者插入标签的方法

```
添加子元素：
  append()和appendTo()：在现存元素的内部，从后面放入元素

  prepend()和prependTo()：在现存元素的内部，从前面放入元素
  
添加兄弟元素：
  after()和insertAfter()：在现存元素的外部，从后面放入元素

  before()和insertBefore()：在现存元素的外部，从前面放入元素
  
// 注意，在开发中基本不会使用到appendTo，prependTo,insertAfter,insertBefore
```



```html
var $span = $('<span>这是一个span元素</span>');
$('#div1').append($span);
......
<div id="div1"></div>
```



##### 06.3-从文档中删除已有标签

```
$('#div1').remove();
```



##### 06.4- 案例：todolist(计划列表)实例

```
案例功能分析：
1. 添加计划    给"增加"按钮绑定点击事件，获取文本框的内容，添加到计划列表
2. 移动计划    控制元素的上下移动[把下一个兄弟获取到，添加到自己的前面]
3. 删除计划    点击"删除"按钮，删除当前一行计划、。
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>todolist</title>
	<style type="text/css">
		.list_con{
			width:600px;
			margin:50px auto 0;
		}
		.inputtxt{
			width:550px;
			height:30px;
			border:1px solid #ccc;
			padding:0px;
			text-indent:10px;			
		}
		.inputbtn{
			width:40px;
			height:32px;
			padding:0px;
			border:1px solid #ccc;
		}
		.list{
			margin:0;
			padding:0;
			list-style:none;
			margin-top:20px;
		}
		.list li{
			height:40px;
			line-height:40px;
			border-bottom:1px solid #ccc;
		}

		.list li span{
			float:left;
		}

		.list li a{
			float:right;
			text-decoration:none;
			margin:0 10px;
		}
	</style>
    <script src="js/jquery-1.12.4.js"></script>
    <script>
    $(function(){
        // 一,添加计划
        // 步骤:
        //   1. 给"增加"按钮绑定点击事件
        $("#btn1").on("click", function(){
            // 2. 获取文本框的内容
//            console.log( $("#txt1").val() );
            var content = $("#txt1").val();
            // 数据的验证
            if(content == ""){
                alert("请输入计划的内容");
                return false;
            }
            // 3. 添加到计划列表
            // 3.1 创建新元素
            var new_li = $('<li><span>'+content+'</span><a href="javascript:;" class="up"> ↑ </a><a href="javascript:;" class="down"> ↓ </a><a href="javascript:;" class="del">删除</a></li>');
            // 3.2 把创建元素添加到计划列表的前面
            $("#list").prepend(new_li);
        });

        // 二,移动计划的上下位置
        // 1. 向下移动
		// 在点击".down"按钮以后，把当前行的li元素与下一行的li元素位置互换
		$("#list").on("click",".down",function(){
		    // 1.1 获取当前行的li元素
            // $(this) 当前被点击的元素[.down]
            // parent()　获取父级元素
		    var current_li = $(this).parent();
		    // 1.2 获取下一行的li元素
			var next_li = current_li.next();
			// 1.3 把下一行li的元素放到当前行的前面
			current_li.before(next_li);
		});

		// 2. 向上移动
		$("#list").on("click",".up",function(){
		   	// 2.1 获取当前行的li元素
			var current_li = $(this).parent();
			// 2.2 获取上一行的li元素
			var prev_li = current_li.prev();
			// 2.3 把上一行的li元素当前当前行的后面
			current_li.after(prev_li);
		});

		// 三,删除计划
		$("#list").on("click",".del",function(){
		    // 删除元素使用remove()
		    $(this).parent().remove();
        });
    });
    </script>
</head>
<body>
	<div class="list_con">
		<h2>To do list</h2>
		<input type="text" name="" id="txt1" class="inputtxt">
		<input type="button" name="" value="增加" id="btn1" class="inputbtn">
		
		<ul id="list" class="list">
			<!-- javascript:; # 阻止a标签跳转 -->
			<li>
				<span>学习html</span>
				<a href="javascript:;" class="up"> ↑ </a>
				<a href="javascript:;" class="down"> ↓ </a>
				<a href="javascript:;" class="del">删除</a>
			</li>
			<li><span>学习css</span><a href="javascript:;" class="up"> ↑ </a><a href="javascript:;" class="down"> ↓ </a><a href="javascript:;" class="del">删除</a></li>
			<li><span>学习javascript</span><a href="javascript:;" class="up"> ↑ </a><a href="javascript:;" class="down"> ↓ </a><a href="javascript:;" class="del">删除</a></li>
		</ul>
	</div>
</body>
</html>
```



总结：

```
1. 获取当前行的父元素
	 $(this).parent();
	 
2. 添加元素到指定标签内部作为子元素
   $(父级元素).prepend(新的li元素);  // 增加计划
3. 上下移动计划的核心：
   向上移动：把当前行与上一行li进行位置互换。
   向下移动：把当前行与下一行li进行位置互换。
4. 删除计划，把当前li删除
   $(this).parent().remove();
```



### 07-使用正则表单式对表单数据进行验证

在上一个阶段中，我们学习了python里面的正则表达式，正则表达式就是一种字符串的高级处理工具，可以让我们根据预定的格式对数据进行复杂的操作，在js中也有正则表达式，他们的格式符号一致。

在上节课的学习中，我们在表单中使用了jQuery对于表单元素的值进行的操作验证，但是针对一些复杂的验证，我们单纯判断数据的长度是不够的，例如，手机号码、邮箱、网页地址的验证，我们还要使用正则表达式来完成。js中的正则表达式主要就是用于验证表单数据。



##### 07.1- 正则表达式的创建

在js中使用正则表达式有两种使用方式：

```javascript
// 正则在js中也是对象。
var re=new RegExp(规则, '可选参数');
var re=/规则/参数;
```



##### 07.2-正则表达式的使用

```javascript
var str ="Hello world";
        //var re=new RegExp(/hel/, 'i');
        var re = /hel/i;
        alert(re.test(str));   //拿着re这个规则去匹配str这个字符串，符合规则就返回true，否则返回false
```



##### 07.3-使用正则表达式验证表单数据

常用的验证表达式：

```
^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$   // 校验email地址
^http://([\w-]+\.)+[\w-]+(/[\w-./?%&=]*)?$      // 校验http协议的url网址
^1[3456789]\d{9}$                              // 国内手机号码
^[\d]{17}[\dxX]$                               // 身份证号码
```



```javascript
//实现用户名的验证
    function checkUser() {
        var reUser = /^\w{6,20}$/;
        var vals = $(this).val();

        // 判断是否为空
        if(vals==""){
            $(this).next().show().html("用户名不能为空！");
            return
        }
        if(reUser.test(vals)){

            //用户输入满足规则了
            $(this).next().hide();

        }else{
            //用户输入不满足规则
            //用户名是6-20位数字、字母和下划线！
            $(this).next().show().html("用户名是6-20位数字、字母和下划线！")
        }
    }
```


