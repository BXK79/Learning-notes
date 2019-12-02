**jQuery篇**

 

#### 一、jQuery概述：

##### 1. JavaScript库：
就是library，是一个封装好的特定的集合（方法和函数），封装了很多预 

先定义好的函数在里面，比如动画animate、hide、show、比如获取元素等。

   简单来说：就是一个js文件，里面对原生js代码进行了封装，利于我们高效使用。

   比如：jQuery，就是为了快速方便的操作DOM，里面基本都是函数。

 

##### 2.jQuery的概念：
把js中的DOM操作做了封装，我们可以快速的查询使用里面的功能。

​                优化了DOM操作、事件处理、动画设计和ajax交互。

 

#### 二、jQuery的基本使用

##### 1. 下载: https://jquery.com/

 

##### 2. 使用步骤：
（1）引入jQuery文件<script src=”jquery.min.js”></script>

(2) 使得DOM加载完毕：$(function（）{jq代码}）

 

##### 3. jQuery的顶级对象$：
$是jQuery的别称，在代码中可以用jQuery代替$。

 

##### 4. jQuery对象和DOM对象：
  用原生js代码获取过来的对象就是DOM对象

​                          用jQuery方式获取来的对象就是jQuery对象

​                          $(‘div’); 是一个jQuery对象（伪数组形式储存）

​                         jQuery 对象只能使用 jQuery 方法，DOM 对象则使用原生的 JavaScirpt 属性和方法。

 

##### 5. DOM对象与jQuery对象之间是可以相互装换的：

（1）DOM>jQuery：$(DOM对象)

（2）jQuery>DOM：$(‘div’)[0]  或者 $(‘div’)get(0) 里面的0是索引号。

 

#### 三、jQuery常用的API

##### 1. jQuery选择器：
$(‘选择器’)   //里面的选择器直接写css选择器即可，需加引号。

 

##### 2. jQuery设置样式：
$(‘div’).css(‘属性’,’值’)

 

##### 3. 隐式迭代：
（比如同时改多个div的样式而不需要for循环）隐式迭代就是把匹配的所有    

元素内部进行遍历循环，给每一个元素添加css这个方法。

 

##### 4. jQuery筛选选择器： 
$("ul li:first").css("color", "red");

​                      $("ul li:eq(2)").css("color", "blue"); 索引号从零开始

​                      $("ol li:odd").css("color", "skyblue"); 奇数

​                      $("ol li:even").css("color", "pink"); 偶数

​                         

##### 5. jQuery筛选方法：
   **父**  parent()  返回的是 最近一级的父级元素 亲爸爸

​            console.log($(".son").parent());

​           当有多级父元素是可以用parents(“.类名”)，可以返回指定的祖先元素。

​            // **2. 子**

​            // (1) 亲儿子 children()  类似子代选择器  ul>li

​            // $(".nav").children("p").css("color", "red");

​           // (2) 可以选里面所有的孩子 包括儿子和孙子  find() 类似于后代选择器

​           $(".nav").find("p").css("color", "red");

​            // **3. 兄**

​                       // 1. 兄弟元素siblings 除了自身元素之外的所有亲兄弟

​            $("ol .item").siblings("li").css("color", "red");

​            /**/ 2. 第n个元素**

​            var index = 2;

​            // (1) 我们可以利用选择器的方式选择

​            // $("ul li:eq(2)").css("color", "blue");

​            // $("ul li:eq("+index+")").css("color", "blue");

​            // (2) 我们可以利用选择方法的方式选择 更推荐这种写法

​            // $("ul li").eq(2).css("color", "blue");

​            // $("ul li").eq(index).css("color", "blue");

​            // **3. 判断是否有某个类名**

​            console.log($("div:first").hasClass("current"));

​            console.log($("div:last").hasClass("current"));

 

##### 6. jQuery里面的排他思想：

​             $(function() {

​            // 1. 隐式迭代 给所有的按钮都绑定了点击事件

​            $("button").click(function() {

​                // 2. 当前的元素变化背景颜色

​                $(this).css("background", "pink");

​                // 3. 其余的兄弟去掉背景颜色 隐式迭代

​                $(this).siblings("button").css("background", "");

​            });

​        })

 

##### 7. 链式编程：
 节省代码量 $(this).css("color", "red").siblings().css("color", "");

 

 

#### 四、jQuery的样式操作

##### 1. 操作css方法：
适用于样式比较简单 

&(‘div’).css(‘width’); 没给值，返回这个属性值，

&(‘div’).css(‘width’，‘300px’); 修改了此属性的值。

&(‘div’).css({

​     Width：400，

​     Height：400

}) 获取css对象同时改多个属性。如果是复合属性采取驼峰命名法，如果值不是数字，则要加引号。

 

##### 2. 设置类样式方法：
适用于修改属性较多时，可以吧css操作写入css文件中。

​       $(function() {

​            // 1. 添加类 addClass()

​            // $("div").click(function() {

​            //     // $(this).addClass("current");

​            // });

​            // 2. 删除类 removeClass()

​            // $("div").click(function() {

​            //     $(this).removeClass("current");

​            // });

​            // 3. 切换类 toggleClass()

​            $("div").click(function() {

​                $(this).toggleClass("current");

​            });

​        })

 

##### 3. Jq的类操作与原生js中的className区别：

​           className会覆盖元素原先里面的类名，jq里面的类操作只是对指定的类进行

操作，不影响原先的类名。 

 

 

#### 五、jQuery的动画效果

##### 1. 显示与隐藏：
语法规范：show([speed,[easing],[fn]]) 显示（以下参数用法相同）

​                         hide([speed,[easing],[fn]]) 隐藏

​                         toggle([speed,[easing],[fn]]) 切换

​              参数： 参数都可以省略

​                 Speed：三种预定速度‘slow’‘normal’‘fast’ 或者动画时长毫秒数（如1000）

​                 easing：指定切换效果，默认‘’swing‘’，可用参数‘linear’

​                 fn：回调函数，在动画完成时执行的函数，每个元素执行一次。

 

##### 2. 滑动效果：
语法规范：slideDown([speed,[easing],[fn]]) 下滑效果（参数用法与显示隐藏                 

一样）

slideUp([speed,[easing],[fn]]) 上滑效果

slideToggle([speed,[easing],[fn]]) 滑动切换

 

##### 3. 事件切换：
语法：hover（[over,]out）

​            参数：over：鼠标移到元素上要出发的函数（相当于mouseenter）

​                  out：鼠标移出元素要触发的函数（相当于mouseleave）

 

​            （1） 事件切换 hover 就是鼠标经过和离开的复合写法

​            // $(".nav>li").hover(function() {

​            //     $(this).children("ul").slideDown(200);

​            // }, function() {

​            //     $(this).children("ul").slideUp(200);

​            // });

​            （2）. 事件切换 hover  如果只写一个函数，那么鼠标经过和鼠标离开都会触发这个函数

​            $(".nav>li").hover(function() {

​                $(this).children("ul").slideToggle();

​            });

 

##### 4. 动画队列及其停止排队方法：

（1）动画或效果队列：动画或效果一旦触发就会执行，若多次触发会造成多个动画或

者效果排队执行。

（2）停止排队：stop（） 用于停止动画效果，把它写到动画或者效果的前面，相当于    

  停止结束上一次的动画。

 $(".nav>li").hover(function() {

​                // stop 方法必须写到动画的前面

​                $(this).children("ul").stop().slideToggle();

​            });

 

##### 5. 淡入淡出效果：
语法：fadeIn([speed,[easing],[fn]]) 淡入

​                      fadeOut([speed,[easing],[fn]]) 淡出

​                      fadeToggle([speed,[easing],[fn]]) 淡入淡出切换（以上这三种参数

与前面显示隐藏一样）

​     fadeTo([speed],opacity,[easing],[fn]) 渐进方式调整到指定的不透明度

参数：那三个同上，其中这里speed必须写。

​     opacity：透明度必须写，取值0~1。

 

##### 6. 自定义动画：

语法：animate(params,[speed],[easing],[fn])

参数：params：想要更改的样式属性，与对象形式传递，必须写。属性名可以不带引号，如果是复合属性则需要采用驼峰命名法如borderLeft。其余参数都可以省略。

​      speed、easing、fn都和前面的一样。

​           $(function() {

​            $("button").click(function() {

​                $("div").animate({

​                    left: 500,

​                    top: 300,

​                    opacity: .4,

​                    width: 500

​                }, 500);

​            })

​        })

 

 

#### 六、jQuery属性操作

##### 1. 设置或获取元素固有属性值prop（）

​     固有属性理解：元素本身自带的属性，比如<a>的href，<input>里的type。

​     语法例子:  $(“a”).prop(“href”) 获取属性值  //prop(“属性”)

​               $(“a”).prop(“title”,”修改了值”)  //prop(“属性”，“属性值”)

 

##### 2. 设置或获取元素自定义属性值 attr（）

​          console.log($("div").attr("index")); 获取自定义的index属性值

​            $("div").attr("index", 4);   设置了自定义的index属性值

​            console.log($("div").attr("data-index")); h5的自定义属性写法

​         补充：另一个可以设置或者获取属性值的方法：数据缓存data（）

​              data（）方法可以在指定的元素上存取数据，并不会修改DOM元素结构，

一旦刷新，之前存放的数据都将被移除。

 数据缓存 data() 这个里面的数据是存放在元素的内存里面

​            $("span").data("uname", "andy");

​            console.log($("span").data("uname"));

​            // 这个方法获取data-index h5自定义属性 第一个 不用写data-  而且返回的是数字型

​            console.log($("div").data("index"));

（案例补充 :checked选择器 :checked查找被选中的表单元素）

 

#### 七、jQuery内容文本值
（普通元素的内容<div>123</div>）

##### 1. 主要针对元素的内容还有表单的值操作

（1）普通元素内容html() （相当于原生innerHTML，连标签一起获取）

​    html()  //获取元素的内容

​    html(“内容”)  //设置元素的内容

（2）普通元素文本内容text() (相当于原生innerText，只获取内容不带标签)

text()  //同上html（）

text(“内容”)  //同上html（“”）

（3）获取设置表单值 val() （相当于原生value）

val()  //同上html（）

val(“内容”)  //同上html（“”）

 

代码示例： // 1. 获取设置元素内容 html()

​        console.log($("div").html());

​        // $("div").html("123");

​        // 2. 获取设置元素文本内容 text()

​        console.log($("div").text());

​        $("div").text("123");

 

​        // 3. 获取设置表单值 val()

​        console.log($("input").val());

​        $("input").val("123");

（案例补充 计算结果保留几位小数 toFixed(2) 保留两位小数）

 

#### 八、jQuery元素操作

##### 1. 遍历元素：
隐式迭代对同一类元素做同样的操作，如想对同一类元素做不同操作就需要

遍历元素 each()方法一。

语法：$(“div”).each(function(index,domEle) {xxx;})

注意：（1）each()方法遍历每一个元素。主要用DOM处理，each每一个

（2）里面的回调函数有两个参数：index是每个元素的索引号；demEle是每个DOM 

元素对象，不是jQuery对象。

（3）所以要想使用jQuery方法，需要给这个DOM元素装换为jQuery对象$（domEle）

  遍历元素方法二：

   语法 $.each(object,function(index,element) {xxx;} )

​    （1）$.each()方法可用于遍历任何对象。主要用于数据处理，比如数组，对象

​    （2）里面的函数有2个参数：index是每个元素索引号，element遍历内容

​    

##### 2. 创建元素：
语法：$(“<li></li>”); 创建了一个<li>

##### 3.  添加元素：
（1）内部添加: element.append(创建的元素); 把内容放入匹配元素内部最后   

面，类似原生appendChild

​         element.prepend(创建的元素); 放入里面的最前面

​            （2）外部添加：element.after(创建的元素) 把内容放入目标元素的后面

​                          element.before(创建的元素) 把内容放入目标元素的前面

##### 4. 删除元素：

   element.remove();  // 删除匹配元素本身

element.empty();  //  删除匹配的元素集合中所有的子节点

element.html(“”);  //  清空匹配的元素内容

 

代码示例： // 1. 创建元素

​            var li = $("<li>我是后来创建的li</li>");

​            // 2. 添加元素

 

​            // (1) 内部添加

​            // $("ul").append(li);  内部添加并且放到内容的最后面 

​            $("ul").prepend(li); // 内部添加并且放到内容的最前面

 

​            // (2) 外部添加

​            var div = $("<div>我是后妈生的</div>");

​            // $(".test").after(div);

​            $(".test").before(div);

​            // 3. 删除元素

​            // $("ul").remove(); 可以删除匹配的元素 自杀

​            // $("ul").empty(); // 可以删除匹配的元素里面的子节点 孩子

​            $("ul").html(""); // 可以删除匹配的元素里面的子节点 孩子

 

​      

#### 九、jQuery尺寸、位置操作

##### 1. jQuery尺寸：          

​     ![img](file:///C:\Users\bbk\AppData\Local\Temp\ksohtml7232\wps1.jpg)

括号里没参数是取值，值为数字型，括号里有参数是赋值，参数可以不写单位。

 

##### 2. jQuery位置：

  位置主要有三个：（1）offset()设置或获取元素偏移

​                   此方法设置或返回被选元素相对于文档的偏移坐标，跟父极没有关系

​                示例：获取设置距离文档的位置（偏移） offset

​                 console.log($(".son").offset());

​                 console.log($(".son").offset().top);

​                  $(".son").offset({

​                   top: 200,

​                   left: 200

​                });

​                  （2）position()获取元素偏移，只能获取不能设置

​                     此方法用于返回被选元素相对于带有定位的父极偏移坐标，如果父极都没有定位，则以文档为准。

​                  console.log($(".son").position());

​                  （3）scrollTop()/scrollLeft()设置或获取元素被卷去的头部

​                           $(function() {

​            $(document).scrollTop(100);

​            // 被卷去的头部 scrollTop()  / 被卷去的左侧 scrollLeft()

​            // 页面滚动事件

​            var boxTop = $(".container").offset().top;

​            $(window).scroll(function() {

​                // console.log(11);

​                console.log($(document).scrollTop());

​                if ($(document).scrollTop() >= boxTop) {

​                    $(".back").fadeIn();

​                } else {

​                    $(".back").fadeOut();

​                }

​            });

​            // 返回顶部

​            $(".back").click(function() {

​                // $(document).scrollTop(0);

​                $("body, html").stop().animate({

​                    scrollTop: 0

​                });

​                // $(document).stop().animate({

​                //     scrollTop: 0

​                // }); 不能是文档而是 html和body元素做动画

​            })

​        })

 

#### 十、jQuery事件

##### 1. 单个事件注册：
语法:  element.事件(function(){ })

​          事件列举：mouseover、mouseout、blur、focus、change、keydown、keyup、resize、scroll等

 

##### 2. 事件处理on()绑定事件:

​          On()方法在匹配元素上绑定一个或多个事件的事件处理函数。

​     语法：  element.on(events,[selector],fn)

（1）events：一个或多个用空格分隔的事件类型，如“click”或“keydown”

（2）selector：元素的子元素选择器。

（3）fn：回调函数 即绑定在元素身上的侦听函数。

​           代码示例：$("div").on({

​            //     mouseenter: function() {

​            //         $(this).css("background", "skyblue");

​            //     },

​            //     click: function() {

​            //         $(this).css("background", "purple");

​            //     },

​            //     mouseleave: function() {

​            //         $(this).css("background", "blue");

​            //     }

​            // });

​      on()方法优势一：可以绑定多个事件，多个处理事件处理程序。

​                     如果事件处理程序相同可以以下方式来写：

​                       $("div").on("mouseenter mouseleave", function() {

​                              $(this).toggleClass("current");

​                          });

​      on()方法优势二：可以事件委派操作，事件委派的定义是：把原来加给子元素身上的事件绑定在父元素身上，就是把事件委派给父元素。

​                 语法示例：$("ul").on("click", "li", function() {

​                             alert(11);

​                          }); //事件绑定在ul上，但是触发事件的对象是ul里面的li

​      on()方法优势三：动态创建的元素，click()没有办法绑定事件，on()可以给动态生成的元素绑定事件。

 

​                  

代码示例： $("ol").on("click", "li", function() {

​                                alert(11);

​                           })

​                             var li = $("<li>我是后来创建的</li>");

​                             $("ol").append(li);

​                           })

 

##### 3. 事件处理off()解绑事件

​        Off()方法可以移除通过on()方法添加的事件处理程序。

$("div").off();  // 这个是解除了div身上的所有事件

$("div").off("click"); // 这个是解除了div身上的点击事件

$("ul").off("click", "li"); // 这个是解除了事件委托的点击事件

特殊：如果有事件只想触发一次，可以使用one()来绑定事件。

​         $("p").one("click", function() {

​                alert(11);

​            })    //只触发一次

 

##### 4. 自动触发事件: 
   有三种

​           （1）元素.事件()

​              $("div").click();  //会触发元素的默认行为

​            （2）元素.trigger("事件")

​               $("div").trigger("click");  //会触发元素的默认行为

​               $("input").trigger("focus");

​            （3）元素.triggerHandler("事件")   //不会触发元素的默认行为

​                $("div").triggerHandler("click");

​                $("input").on("focus", function() {

​                $(this).val("你好吗");

​              });

 

##### 5. jQuery事件对象：
事件被触发，就会有事件对象的产生。

​       element.on(events,[selector], function(even) { })   //其中的even就是事件对象

​      阻止默认行为：event.preventDefault()或者return false

​      阻止冒泡：event.stopPropagation()

​         代码示例： $(function() {

​            $(document).on("click", function() {

​                console.log("点击了document");

 

​            })

​            $("div").on("click", function(event) {

​                console.log("点击了div");

​                event.stopPropagation();

​            })  })

 

#### 十一、jQuery其他方法

##### 1. jQuery对象拷贝：
如果想要把某个对象拷贝(合并)给另一个对象使用，此时可以使用 

$.extend()方法

语法：&.extend([deep],target,object1,[objectN])

（1）deep：如果设为true为深拷贝，默认为false浅拷贝。

（2）target：要拷贝的目标对象

（3）object1：待拷贝到第一个对象的对象

语法示例： var targetObj = {};

​            // var obj = {

​            //     id: 1,

​            //     name: "andy"

​            // };

​            // $.extend(targetObj, obj);

​            // console.log(targetObj);

注意：当目标对象中有同属性名的会覆盖原来的数据

​        例如： // var targetObj = {

​            //     id: 0

​            // };

​            // var obj = {

​            //     id: 1,

​            //     name: "andy"

​            // };

​            // // $.extend(target, obj);

​            // $.extend(targetObj, obj);

​          // console.log(targetObj); // 会覆盖targetObj 里面原来的数据

 

##### 2. 浅拷贝和深拷贝区别理解：

​         浅拷贝是把被拷贝的对象复杂数据类型中的地址拷贝给目标对象，修改目标对象会影响被拷贝对象。

![img](file:///C:\Users\bbk\AppData\Local\Temp\ksohtml7232\wps2.jpg) 

​                                      （浅拷贝）

 

​        深拷贝，前面加true，完全克隆（拷贝对象，而不是地址），修改目标对象不会影响被拷贝对象，如果里面有不冲突的属性,会合并到一起 。

![img](file:///C:\Users\bbk\AppData\Local\Temp\ksohtml7232\wps3.jpg) 

（深拷贝，msg里面有不冲突的属性，会和原有的合并而不是覆盖）

 

##### 3. jQuery多库共存

   问题描述：jQuery使用$作为标识符，随着jq的流行，其他js库也会用$作为标识符，这样一起使用会引起冲突。

   客观需求：需要一个解决方案，让jQuery和其他的js库不存在冲突，可以同时存在，这就叫多库共存。

  jQuery解决方案：（1）把里面的$符号统一改为jQuery。比如jQuery(“div”)

​                 （2）jQuery变量规定新的名称：$.noConflict() 如var xx = $.noConflict();

  

##### 4. jQuery插件

​     插件依赖于jQuery，所以必须要先引入jQuery文件。

​     jQuery插件常用的网站：（1）jQuery插件库http://www.jq22.com/

​                          （2）jQuery之家 http://www.htmleaf.com/

​     jQuery插件使用步骤：（1）引入相关文件。（jQuery文件 和 插件文件）

​                        （2）复制相关html、css、js（调用插件）。

 

##### 5. 图片懒加载插件：
图片使用延迟加载可提高网页下载速度，它也能帮助减轻服务器负载。

​               当我们页面滑动到可视区域，再显示图片。

​          使用EasyLazyload注意此时的js引入文件和js调用必须写到DOM元素（图片）最后面。

  （案例补充小技巧：当要大量替换相同属性名或者啥的可以按CTRL+H再操作。）

 

##### 6. 全屏滚动（fullpage.js）

gitHub: https://github.com/alvarotrigo/fullPage.js

中文翻译网站：http://www.dowebok.com/demo/2014/77/

 

##### 7. bootstrap JS插件

  他也是依赖于jQuery开发的，因此里面的插件使用必须引入jQuery文件。

