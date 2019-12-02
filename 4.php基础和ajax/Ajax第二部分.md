#### 总结回顾

框架图：
![Screenshot_2019-10-30-21-54-39-939_tv.danmaku.bil](F:\js笔记用到的图片\Screenshot_2019-10-30-21-54-39-939_tv.danmaku.bil.png)

回调函数本质：让我们可以为一个封装体传入一些逻辑

#### 封装Ajax

##### get方式

![Screenshot_2019-10-30-22-34-23-471_tv.danmaku.bil](F:\js笔记用到的图片\Screenshot_2019-10-30-22-34-23-471_tv.danmaku.bil.png)

##### post方式略

##### get和post一起封装

~~~javascript
/**
 * 参数越来越多之后 用户如果要传递参数 必须遵循
 * @param {*} url 
 * @param {*} type 
 * @param {*} data 
 * @param {*} success 
 */
function ajax_test(url, type, data, success) {
  var xhr = new XMLHttpRequest();
  // 如果是get 并且有数据
  if (type == 'get' && data) {
    url += '?';
    url += data;
    data = null; // 这样最后直接send data即可 
  }
  xhr.open(type, url);
  // post请求 并且有数据
  if (type == 'post' && data) {
    xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
  }
  xhr.onreadystatechange = function () {
    if (xhr.readyState == 4 && xhr.status == 200) {
      success(xhr.responseText);
    }
  }
  xhr.send(data);
}
~~~

调用：

~~~javascript
 // ajax 自选get post
  document.querySelector('.ajax_test').onclick = function(){
    // ajax_test('../00.backData/01.backSendData.php','get','name=葫芦娃&food=胡萝卜',function(data){
    //   alert('葫芦娃');
    //   alert(data);
    // })
    ajax_test('../00.backData/01.backSendData.php','post','name=蛇精&food=爷爷',function(data){
      alert('蛇精');
      alert(data);
    })
  }
~~~

##### get和post一起封装改进

以上方法：参数越来越多之后 用户如果要传递参数 必须遵循顺序，参数多了之后比较难搞
                  所以采用对象方式来传递参数

并判断和处理xml和json

~~~javascript
// 只传递一个参数
// 用户传入的是 对象
/*
  键名
    url
    type
    data
    success
  用户不需要记忆 参数的顺序 只需要记忆 参数的属性名即可
  大部分的框架 都是这么做的
*/
function ajax(option) {
  var xhr = new XMLHttpRequest();
  // 如果是get 并且有数据
  if (option.type == 'get' && option.data) {
    option.url += '?';
    option.url += option.data;
    option.data = null; // 这样最后直接send data即可 
  }
  xhr.open(option.type, option.url);
  // post请求 并且有数据
  if (option.type == 'post' && option.data) {
    xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
  }
  xhr.onreadystatechange = function () {
    if (xhr.readyState == 4 && xhr.status == 200) {
      // option.success(xhr.responseText);
      // console.log(xhr.getResponseHeader('Content-Type'));
      var type = xhr.getResponseHeader('Content-Type');
      // 是否为json
      if (type.indexOf('json') != -1) {
        option.success(JSON.parse(xhr.responseText));
      }
      // 是否为xml
      else if (type.indexOf('xml') != -1) {
        option.success(xhr.responseXML);
      }
      // 普通字符串
      else {
        option.success(xhr.responseText);
      }
    }
  }
  xhr.send(option.data);
}
~~~

调用：

~~~javascript
// ajax 只传递一个参数
  document.querySelector('.ajax').onclick = function(){
    ajax({
      type:'get',
      data:'skill=爱吃西兰花和芹菜蹦蹦跳跳好可爱&name=兔子',
      success:function(data){
        document.body.innerHTML = data;
      },
      url:'../00.backData/01.backSendData.php'
    });
  }

 // 获取json
  document.querySelector('.json').onclick = function(){
    ajax({
      type:'get',
      url:'../00.backData/backJSON.php',
      success:function(data){
        console.log(data);
      }
    })
  }

  // 获取 xml
    document.querySelector('.xml').onclick = function(){
    ajax({
      type:'post',
      url:'../00.backData/backXML.php',
      success:function(data){
        console.log(data);
      }
    })
  }
~~~

*总结*

  *封装的目的*

   *让我们把精力集中在逻辑*

   *页面的交互效果*

   *基础的部分不用每次都来一遍*

  *封装的步骤*

   *固定的部分 抽取*

   *不固定的部分 作为参数*

   *参数很多==>*

​    *使用对象 来优化*

  *封装的好坏*

   *功能能否正常执行*

   *代码的简洁程度(可读性)*

   *考虑的问题是否足够多,兼容性问题,异常处理*

#### jQuery中的Ajax使用

只要导入jQuery包就可以直接使用，语法：$.ajax() 里面的参数具体看下

~~~javascript
 $.ajax使用补充
  $('.ajax_extend').click(function(){
    /*
      type 如果不设置 默认的请求方法是? get
        如果用get请求 那么 可以不写 type属性
      success
        在请求成功才会触发
        如果出现 服务器告诉浏览器返回的类型 跟 jQuery内部实际转换的类型不匹配 会无法触发
      error
        当请求出现问题 会触发这个回调函数
      complete
        请求完成会触发
    */
    $.ajax({
        // url:'../00.backData/01.backSendData.php',
        // data:{s
        //   name:'喜洋洋',
        //   skill:'卖萌'
        // },
        // url:'../00.backData/backXML.php',
        url:'../00.backData/backJSON.php',
        success:function(data){
          console.log(data);
        },
        // 形参可以改名字 这里直接使用默认的即可
        // 参数1 异步对象
        // 参数2 错误信息
        // 参数3 错在哪里
        error:function(XMLHttpRequest, textStatus, errorThrown){
          console.log('失败了哦');
          console.log(XMLHttpRequest);
          console.log(textStatus);
          console.log(errorThrown);
        },
        complete:function(){
          console.log('请求完成了');
        }
    })
  })
~~~

#### 模板引擎的使用

这里使用的是：在GitHub上搜索art-template
导入这个js文件：

            <script  src="./template-web.js"></script>

1.定义模板

  type 不写 或者是 text/javascript 会被解析为 js

  如果写成其他的内容 不会被当做js解析-->

<script type='text/html' id='template'>

2.挖坑

~~~html
<script type='text/html' id='template'>
  <ul>
    <li>名字{{name}}</li>
    <li>技能{{skill}}</li>
    <li>爱好{{habbit}}</li>   //{{}}就是挖坑起名字
  </ul>
</script>
~~~

3.填坑

~~~javascript
<script>
  var data = {
    name:'彭林',
    skill:'约跑',
    habbit:'跑的一手好步'
  };

  // 填坑
  // 参数1 模板的id
  // 参数2 填充的数据
  var result = template('template',data);
  console.log(result);
  document.body.innerHTML = result;
</script>
~~~

例子：地址：F:\前端学习\ajax\04-{工具函数、jQueryAjax、模板引擎}\4-源代码\2017-8-15\08.luowang

~~~javascript
<body>
	<div class="left-control">
		<h2>加载一张</h2>
		<div id='getMore' class="icon-download icon-4x"></div>
		<!--<div id='getMore' class="icon-spinner icon-spin icon-4x"></div>-->
		<h2>加载多张</h2>
		<div id='getSome' class="icon-download icon-4x"></div>
	</div>
	<div class="container">
		<div class="item">
			<a href="#" class='cover'><img src="images/vol.859.jpg" alt=""></a>
			<div class="bottom">
				<a href="#">vol.847 用一首歌来想象你</a>
				<div class='rightBox'>
					<span class='icon-heart'>18554</span>
					<span class='icon-comment'>292</span>
				</div>
			</div>
		</div>
	</div>
</body>

</html>

<!-- 1.使用jQuery 发送ajax请求 -->
<script type="text/javascript" src="./js/jquery-1.12.4.min.js"></script>
<!--
		步骤
			0.导入模板引擎
			1.定义模板
			2.挖坑 起名字
				挖坑 一般是取决于 数据
			3.填坑
				数据 服务器 ajax
				回调函数
			4.使用
	-->
<!--  导入模板引擎  -->
<script src='./js/template-web.js'></script>
<!-- 定义模板  -->
<script type='text/html' id='template'>
	
	<div class="item">
				<a href="#" class="cover"><img src="{{path}}" alt=""></a>
				<div class="bottom">
					<a href="#">{{name}}</a>
					<div class="rightBox">
						<span class="icon-heart">{{star}}</span>
						<span class="icon-comment">{{message}}</span>
					</div>
			</div>
	</div>
</script>

<script>
	// 获取数据
	$(function(){
		$('#getMore').click(function(){
			$.ajax({
				url:'_api/luowang.php',
				data:{
					index:3
				},
				success:function(data){
					console.log(data);
					// 填坑
				 var result =	template('template',data.item);
					console.log(result);
					$('.container').append(result);
				}
			})
		})

		// 获取多条
		$('#getSome').click(function(){
			$.ajax({
				url:'_api/luowang_getSome.php',
				data:{
					num:Math.floor((16*Math.random()))
				},
				success:function(data){
						console.log(data);
						// 循环 填坑
						for(var i =0;i<data.items.length;i++){
							// 填坑 使用
							$('.container').append(template('template',data.items[i]));
						}
				}
			})
		})
	})
</script>
~~~

#### 一些接口的调用

1. http://api.asilu.com/#geo 
        在线jsonp接口测试，里面有各种各样的数据，比如天气查询，手机归属地，历史上的今天....等



#### jquery.ajax使用补充

##### 1.dataType：

  可以设置接收返回数据的类型。比如接收json数据的PHP代码中没有设置header，那么$.ajax是无法解析的，只会显示字符串结果，这时可以在js中写   dataType：‘json’；  来解决。xml也类似。

##### 2.ajax事件

   ajaxStart()   ajaxComplete()等，具体和其他一些功能查阅官网手册，直接上例子：

~~~javascript
<script src="./jquery-1.12.4.min.js"></script>
<!--  
    遮罩层
       开启 发送请求时
       关闭 请求回来时
  -->
<script>
  $(function () {
    $('.json').click(function () {
      // 开启
      $.ajax({
        url: '../00.backData/backJSON.php',
        success: function (data) {
          console.log(data);
          // 关闭
        },
        // 人为的指定 返回的数据格式
        dataType: 'json'
      })
    })
    $('.XML').click(function () {
      // 开启
      $.ajax({
        url: '../00.backData/backXML.php',
        success: function (data) {
          console.log(data);
          // 关闭
        },
        // 人为的指定 返回的数据格式
        dataType: 'xml'
      })
    })
  })

  //
  // $(".cover").ajaxStart(function(){
  $(document).ajaxStart(function () {
    $('.cover').show();
  });
  $(document).ajaxComplete(function () {
    $('.cover').hide();
  });
  /*
    离线文档 可能有错误
      去官方文档查询最新的 使用方式
  */ 
</script>
~~~

#### 自己实现模板引擎

 *模板引擎原理*

  *通过id 获取 模板中的 内容 ==>字符串*

  *调用方式时* 

   *找到字符串中 特殊的符号{{name}} ==>正则表达式*

   *使用对象对应的属性 进行替换*

  *返回字符串*

~~~javascript
// 抽取 公共的
// 不确定的 作为参数
// 结果 返回给用户
// 
function my_template(id,data){
  // 字符串
  var str = document.querySelector('#'+id).innerHTML;
  // 定义正则
  var reg = /{{(\w+)}}/;
  // 循环替换 直到 为null 结束
  var result = reg.exec(str);
  while(result){
    str = str.replace(result[0],data[result[1]]);
    result = reg.exec(str);
  }
  // 获取结果
  return str;
}
~~~

调用

~~~javascript
<script id='template' type="text/html">
  <ul>
    <li>{{name}}</li>
    <li>{{skill}}</li>
  </ul>
</script>
<!--  引入模板引擎  -->
<script  src="./my_template.js"></script>
<script>
var data = {
    name:'王宝强',
    skill:'找了个好经纪人'
  }
  var resultString = my_template('template',data);
  console.log(resultString);
</script>
~~~

*模板引擎封装总结*

  *让大伙了解模板引擎的 核心原理*

   *使用 正则表达式检索字符串 直到检索不到为止*

 *总结--面试官问到*

   *模板引擎==>用过*

   *给我说说呗==>所以说用法 说完用法 说原理,我自己试着实现过一次*



##### 模拟面试

 *ajax*

  *用过ajax吗?*

​    *用过,用在不刷新页面获取数据*

​    *用法是 创建异步对象 请求行 请求头 回调函数 请求主体*

​    *请求响应回来之后 会触发 回调函数 而我们渲染页面的操作就是写在回调函数中*

​    *在我写的项目里面 结合ajax 基本是这么用的*

​      *发送之前 修改页面结构* 

​      *数据回来之后 修改页面结构*

​    *我从后台拿到的数据一般是 JSON,曾经碰到过 用XML的不知道 贵公司 用的是 哪种方式交互数据*

​    *每次都自己写 好烦,*

​     *试着自己封装了ajax*

​     *可以设置get post data url success*

​    *后来发现 jQuery有ajax 开始使用 jQuery的ajax*

​     *jQuery的ajax 中帮我们实现了 自动转化数据的操作*

​    *试着 修改了一下 自己封装的 ajax工具函数也实现了 自动转化数据的功能*

​     *原理就是 获取 返回的 content-type 做判断即可*

​    *所以我在项目中 如果用了 jQ 直接使用Jq的ajax 如果不需要用jQ 那么我就用自己封装的 Ajax*

​    *随着数据的复杂程度变高 我发现修改dom元素很讨厌*

​     *找到了 模板引擎这个东西 对比了一下 用 art-template*

​     *用了一段时间之后 感觉不错就去试着自己实现了以下*

​      *原理是 正则 替换文本*

#### art-template模板引擎用法补充

~~~javascript
<!-- 导入模板引擎 -->
<script src="./js/template-web.js"></script>
<!-- 逻辑语句 条件 -->
<script id='ifTemplate' type="text/html">
  <ul>
    {{if male=='女'}}
    <li>欢迎您 {{name}} 女士
      <ol>
        <li>这是最新款的包包</li>
        <li>这是最新款的口红</li>
        <li>没想到,你竟然是{{skill}}</li>
      </ol>
    </li>
    {{else if male=='男'}}
      <li>欢迎您 {{name}} 先生
        <ol>
          <li>这是最新款的拖拉机</li>
          <li>讨厌,才来找人家</li>
          <li>没想到,你竟然稍长{{skill}}</li>
        </ol>
      </li>
    {{/if}}
  </ul>
</script>
<script>
  var person1 = {
    male: '女',
    name: '郑爽',
    skill: '身材好'
  };
  var person2 = {
    male: '男',
    name: '张翰',
    skill: '这篇鱼塘我承包了'
  };
  console.log(template('ifTemplate', person1));
  console.log(template('ifTemplate', person2));
</script>
<!-- 原文输出 -->
<script id='norTemplate' type="text/html">
  <ul>
    <li>{{name}}</li>
    <li>{{skill}}</li>
    <li>{{@info}}</li>
  </ul>
</script>
<script>
  var person = {
    name:'犬夜叉',
    skill:'变犬',
    info:'<a href="https://baike.baidu.com/item/%E7%8A%AC%E5%A4%9C%E5%8F%89/26878?fr=aladdin">犬夜叉</a>'
  }
  document.body.innerHTML = template('norTemplate',person);

</script>
<!-- 循环语句 -->
<script id='eachTemplate' type="text/html">
  <ul>
    <li>{{name}}</li>
    <li>兄弟们
        {{each brother}}
          <li>{{$value}}</li>
        {{/each}}
    </li>
    <li>家人们
      <ol>
        {{each family}}
        <li>{{$value.name}}---{{$value.skill}}</li>
        {{/each}}
      </ol>
    </li>
  </ul>
</script>
<script>
   var person = {
     name:'大娃',
     brother:[
       '二娃',
       '三娃',
       '水娃',
       '火娃',
       '千里眼娃',
       '瓜娃子'
     ],
     family:[
       {name:'爷爷',skill:'被抓'},
       {name:'穿山甲',skill:'到底说了什么'},
       {name:'小蝴蝶',skill:'撩--葫芦娃'}
     ]
   }

   console.log(template('eachTemplate',person));

</script>
~~~

#### jsonp接口使用

##### 什么是同源

​    例如：http://127.0.0.1/2017-8-17/09.waterFall_ajax/
​               http://127.0.0.1/2017-8-17/09.waterFall_ajax/api/waterFall.php
​      1.协议名:http
​      2.地址一样
​      3.端口号：默认是80端口
上述三个条件一样 称之为 同源

不同源
    协议,地址,端口号 有一个不一样 称之为不同源

##### 什么是跨域

​       不同源的网站之前 互相发送请求

![qq_pic_merged_1572613958692](F:\js笔记用到的图片\qq_pic_merged_1572613958692.jpg)

##### cors：

​     跨域的一种方法，在后台设置允许跨域就行了
​    有后台程序员来设置，但此方法只能用于HTML5

  ~~~
<?php 
  // 设置允许跨域访问
  // 有后台程序员来设置
  header('Access-Control-Allow-Origin: *');
   echo '你来了呀';
?>
  ~~~

~~~
<script>
  document.querySelector('input').onclick = function(){
    var xhr  = new XMLHttpRequest();
    xhr.open('get','http://192.168.70.78/2017-8-17/11.cross/backData.php');
    xhr.onreadystatechange = function(){
      if(xhr.readyState==4&&xhr.status==200){
        console.log(xhr.responseText);
      }
    }
    xhr.send(null);
  }

</script>
~~~

##### JSONP

![02](F:\前端学习\ajax\05-Ajax{模板引擎、瀑布流、跨域、JSONP}\2-其他资料\02.png)

~~~
 <script>
       function doSomething(data){
         console.log(data);
       }
   </script>
   <script  src="http://192.168.70.78/2017-8-17/12.JSONP/backData.php?callback=doSomething">

      doSomething({"name":"jack","food":"西兰花"})
   </script>
~~~

~~~
<?php 
  // echo 'console.log("数据给你,拿去")';
  // doSomething
  $methodName = $_GET['callback'];

  // 把数据 拼接到 函数名的后面
  echo $methodName.'({"name":"jack","food":"西兰花"})';
?>
~~~

JSONP面试常问的问题：
     JSONP的原理：利用script的src发送跨域请求，服务器返回一个方法的调用，并且返回数据。
     JSONP能不能发post请求：不能，数据是拼在url后面的
     JSONP跟ajax有没有关系：没有
     jQuery中的JSONP跟ajax有没有关系：也没有，jQuery中只是为了方便调用，但是跟ajax没有关系

#### jQuery格式化表单数据

  提供了把一个form表单数据一次性提交到后台的方法，不用在一个个提交

~~~
<body>
  <h2>jQuery格式化表单数据</h2>
  <form action="">
    <input name='name' type="text" placeholder="用户名">
    <input name='tel' type="text" placeholder="电话">
    <input name='email' type="text" placeholder="邮箱">
    <input name='address' type="text" placeholder="地址">
  </form>
</body>
</html>
<script  src="./jquery-1.12.4.js"></script>
<script>
  $(function(){
    $('h2').click(function(){
      var data = $('form').serialize();
      console.log(data);2
    })
  })
</script>
~~~

#### XHR : AJAX提供的一种上传文件到服务器的方法

  因为兼容性问题，使用的不多，了解即可

   *1. 快速格式化表单数据*

   *2. ajax上传文件*

   *3. 上传进度监控*

~~~javascript
<form action="">
    <input type="text" name="name" placeholder="用户名">
    <input type="text" name="email" placeholder="邮箱">
    <input type="text" name="address" placeholder="地址">
    <input type="file" name="icon">
  </form>
  <input type="button" value='ajax发送数据'>
  <div class="progress">
    <div class="step"></div>
  </div>
</body>

</html>
<script>
  // 点击事件
  document.querySelector('input[type=button]').onclick = function () {
    // ajax请求
    var xhr = new XMLHttpRequest();
    // 快速格式化表单数据 要使用 post
    xhr.open('post', 'backData.php');
    // 如果使用自动化表单 不需要设置请求头
    // 回调函数
    xhr.onreadystatechange = function () {
      if (xhr.readyState == 4 && xhr.status == 200) {
        console.log(xhr.responseText);
      }
    }
    // 格式化表单
    // 兼容性问题 使用频率 不是很高了解的知识点
    var sendData = new FormData(document.querySelector('form'));

    // 上传回调函数
    xhr.upload.onprogress = function (event) {
      // console.log(event);
      var percent = event.loaded / event.total * 100 + '%';
      console.log(percent);
      document.querySelector('.step').style.width = percent;
    }

    xhr.send(sendData);
  }
</script>
~~~

#### 到ajax以前知识点框架总结

![1](F:\js笔记用到的图片\ajax之前知识点框架\1.png)

![2](F:\js笔记用到的图片\ajax之前知识点框架\2.png)

![3](F:\js笔记用到的图片\ajax之前知识点框架\3.png)

![4](F:\js笔记用到的图片\ajax之前知识点框架\4.png)

![5](F:\js笔记用到的图片\ajax之前知识点框架\5.png)

![6](F:\js笔记用到的图片\ajax之前知识点框架\6.png)

![7](F:\js笔记用到的图片\ajax之前知识点框架\7.png)