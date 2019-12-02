                                                                           ###                                                                                                                                ajax笔记



#### 了解报文

![Screenshot_2019-10-25-15-52-06-003_tv.danmaku.bil](F:\js笔记用到的图片\Screenshot_2019-10-25-15-52-06-003_tv.danmaku.bil.png)

![Screenshot_2019-10-25-15-51-41-041_tv.danmaku.bil](F:\js笔记用到的图片\Screenshot_2019-10-25-15-51-41-041_tv.danmaku.bil.png)![Screenshot_2019-10-25-15-44-13-606_tv.danmaku.bil](F:\js笔记用到的图片\Screenshot_2019-10-25-15-44-13-606_tv.danmaku.bil.png)![Screenshot_2019-10-25-15-49-33-535_tv.danmaku.bil](F:\js笔记用到的图片\Screenshot_2019-10-25-15-49-33-535_tv.danmaku.bil.png)![Screenshot_2019-10-25-15-49-00-631_tv.danmaku.bil](F:\js笔记用到的图片\Screenshot_2019-10-25-15-49-00-631_tv.danmaku.bil.png)

#### XMLHTTPRequest的基本使用

~~~
<script>
  // 绑定点击事件
  document.querySelector('input').onclick = function(){
      // 创建 对象 异步对象
      var xhr = new XMLHttpRequest();

      // 请求行 
      // get请求 数据是拼接在 url中
      // xxx.php?key=value&key2=value2
      xhr.open('get','xxx.php?name=jack&skill=painting');


      // 注册回调函数
      // 请求响应回来之后触发
      xhr.onload = function(){
        console.log('请求响应回来啦');
        console.log(xhr.responseText);
        alert(xhr.responseText);
      }


      // 请求头 setRequestHeader
      // 参数1 键名
      // 参数2 值
      // 目前这个好像没有任何的作用 是否可以省略呢?
      // get请求 可以省略 设置请求头的操作
      // xhr.setRequestHeader('heima','goodgoodstudy day day up');

      // 请求主体 发送
      xhr.send(null);
  }
</script>
~~~

完整例子，按照步骤来 ajax最基本操作，不跳转页面与服务器交换数据

~~~
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>title</title>
</head>
<body>
  <h2>点击的时候发送请求报文 --不刷新页面</h2>
  <input type="button" value='get请求'>
  <h3></h3>
</body>
</html>
<script>
  // 点击事件
  document.querySelector('input').onclick = function(){
    //1.创建对象
    var xhr = new XMLHttpRequest();

    //2.设置请求行(get请求数据写在url后面)
    xhr.open('get','getData.php?name=rose&skill=swim');

    //3.设置请求头(get请求可以省略,post不发送数据也可以省略)
    // xhr.setRequestHeader()

    //3.5注册回调函数
    xhr.onload = function(){
      // 获取数据
      console.log(xhr.responseText);
      // 修改页面的dom元素
      document.querySelector('h3').innerHTML = xhr.responseText;
    }
    //4.请求主体发送(get请求为空，或者写null，post请求数据写在这里，如果没有数据，直接为空或者写null)
    xhr.send(null);
  }
</script>
~~~

~~~
<?php 
  // 原封不动的返回 发送过来的数据
  print_r($_GET);
  // 延迟一会
  sleep(2);
 ?>
~~~

#### 验证用户名密码案例

步骤

1. 浏览器端
   input type = test
   失去焦点事件
    不刷新页面的情况下发送请求 ---ajax
    xxx.php?name=inputDom.value
    .onload
    .send()
2. 服务器端
   接收提交的数据 $_GET
   假数据模拟 =>数组
   判断是否在数组中存在
   返回不同的内容给用户即可
     恭喜你可以用
     很遗憾用不了
3. 渲染到页面上
   找到一个标签 修改内容即可

html

~~~
<body>
  <h2>用户注册</h2>
  <h3>状态</h3>
  <!--  name属性不是必须的 form表单才是必须的  -->
  <!-- <input type="text" name='userName' placeholder="请输入用户名"> -->
  <input type="text" placeholder="请输入用户名">
</body>

</html>
<script>
  // 什么时候发送请求?
  document.querySelector('input').onblur = function () {
    // 0.修改页面的提示信息
    document.querySelector('h3').innerHTML = '验证中';
    //1.创建对象
    var xhr = new XMLHttpRequest();
    //2.设置请求行(get请求数据写在url后面)
    xhr.open('get', 'checkName.php?name='+this.value);
    //3.设置请求头(get请求可以省略,post不发送数据也可以省略)
    //3.5注册回调函数
    xhr.onload = function(){
      console.log(xhr.responseText);
      document.querySelector('h2').innerHTML = xhr.responseText;
      // 修改提示信息
      document.querySelector('h3').innerHTML = '验证完毕';
    }
    //4.请求主体发送(get请求为空，或者写null，post请求数据写在这里，如果没有数据，直接为空或者写null)
    xhr.send(null);
  }
</script>
~~~

PHP

~~~
<?php 
  //  接收提交的数据 $_GET
  $name = $_GET['name'];

  //   假数据模拟 =>数组
  $nameArr = array('jack','rose','icemountain','Robot');
  //   判断是否在数组中存在
  // 参数1 查询的内容
  // 参数2 去哪个数组查询
  $result = in_array($name,$nameArr);

  //   返回不同的内容给用户即可
  if($result == true){
      echo '很遗憾,已被注册,再选一个吧.o(╯□╰)o';
  }else{
      echo '恭喜你,还没人用呢!!快点注册哦 O(∩_∩)O哈哈~';
  } 
  //     恭喜你可以用
  //     很遗憾用不了
  sleep(2);
 ?>
~~~

#### ajax用post发送数据

必须写请求头setrequestheard

发送数据写在send（）中

~~~
<body>

  <h2>ajax发送post请求</h2>
  <input type="button" value='post请求'>

</body>

</html>
<script>
  // 点击事件
  document.querySelector('input').onclick = function(){
    //1.创建对象
    var xhr = new XMLHttpRequest();

    //2.设置请求行(get请求数据写在url后面)
    xhr.open('post','postData.php');

    //3.设置请求头(get请求可以省略,post不发送数据也可以省略)
    // 可以在 w3c的 ajax分类中 w3c的 form表单的 enctype中 看到 这个值
    xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded");

    //3.5注册回调函数
    xhr.onload = function(){
      console.log(xhr.responseText);
    }
    //4.请求主体发送(get请求为空，或者写null，post请求数据写在这里，如果没有数据，直接为空或者写null)
    // post请求发送数据 写在send中
    // key=value&key2=value2
    xhr.send('name=西兰花&friend=鸡蛋');
  }

</script>
~~~

~~~
<?php 
  // 返回内容
  echo '你 post 过来了';
  // 获取post的数据
  print_r($_POST);
 ?>
~~~

#### 同步与异步

同步：一件事一件事的干

异步：同时干很多事情

​    open（）
​    // 参数3 是否使用异步 默认是 true  false(同步)
​    // 同步 请求响应回来之前 什么事都干不了
​    // 基本上 不会使用这个模式 知道有这个选项即可
​    xhr.open('get', 'xxx.php', false);

#### onload与onreadystatechenge两种回调函数比较

​    onreadystatechenge注册回调函数
​    // 触发的次数很多  
​    // 状态改变时 会触发
​    xhr.onreadystatechange = function () {
​      // 只有当 状态码是 4时 采取获取数据
​      console.log(xhr.status);
​      // 判断 响应回来 并且请求的页面存在 采取获取数据
​      if (xhr.readyState == 4&&xhr.status==200) {
​        console.log('我触发了');
​        console.log(xhr.readyState);
​        console.log(xhr.responseText);
​      }

补充面试官 叫你手写 ajax
      尽可能用电脑写 自己的 他们的
      写哪个都可以
        onload 面试官追问 什么鬼 
        还有 onreadystatechange
      如果要手写 onreadystatechange事件不驼峰命名

#### 了解xml

申明头：是固定的可写可不写

~~~
<?xml version="1.0" encoding="UTF-8"?>
~~~

xml里的标签可以任意写如：<genjiedian></genjiedian>，xml里只有双标签。

PHP要设置header('content-type:text/xml;charset=utf-8');  HTML中才能拿到返回的dom文件

ajax获取xml文件例子

~~~
xml：
<?xml version="1.0" encoding="UTF-8"?>
<root>
  <name>周林林</name>
  <age>18</age>
  <skill>唱歌</skill>
</root>

php：
<?php 
  // 告诉浏览器 返回的是 xml 编码格式是 utf-8
  header('content-type:text/xml;charset=utf-8');
  // 接收发送过来的数据

  // 读取 xml
  // =>哪个分类中 文件分类中找
  // 参数1 文件的路径名
  $xmlString = file_get_contents('data/person.xml');

  // 返回读取的 xml
  echo $xmlString;
 ?>
 
 html：
 <!DOCTYPE html>
<html lang="zh-CN">

<head>
  <meta charset="UTF-8">
  <title>title</title>
</head>

<body>
  <h2>请求xml文件</h2>
  <input type="button" value='ajax获取XML文件'>
  <h3></h3>
</body>

</html>
<script>
  // 点击 请求xml
  document.querySelector('input').onclick = function () {
    //1.创建异步对象
    var xhr = new XMLHttpRequest();
    //2.设置请求行
    xhr.open('post', 'backXML.php');
    //3.设置请求头(get请求可以省略)
    //4.注册状态改变事件
    xhr.onreadystatechange = function () {
      //4.1判断状态&请求是否成功并使用数据
      if (xhr.readyState == 4 && xhr.status == 200) { 
        // 返回的是 XML 通过responseText 只能够获取到 字符串
        console.log(xhr.responseText);
        // 如果返回的是 xml 使用responseXML来获取
        console.log(xhr.responseXML);
        // 页面中默认的文档对象
        console.log(document);

        // 解析 通过responseXML 这个 文档对象 来解析
        console.log(xhr.responseXML.querySelector('name').innerHTML);
        console.log(xhr.responseXML.querySelector('age').innerHTML);
        console.log(xhr.responseXML.querySelector('skill').innerHTML);

        // 获取信息
        var name = xhr.responseXML.querySelector('name').innerHTML;
        var age = xhr.responseXML.querySelector('age').innerHTML;
        var skill = xhr.responseXML.querySelector('skill').innerHTML;

        // 设置给dom元素
        document.querySelector('h3').innerHTML = name+'--'+age+'--'+skill;

      }
    }
    //5.发送请求
    xhr.send(null);
  }
</script>
~~~

 xml的缺点：
        传输的数据量大 ==>现在的网络这么好 这个不是最主要的缺点
        解析略微有点复杂 
    目前使用最频繁的是 JSON

#### JSON基本使用

   1.JSON是一种数据的格式
    2.JSON跟编程语言没有关系
    3.JSON的载体是字符串
    4.基本上所有的编程语言都支持JSON
    5. 语法简洁 基本上所有的编程语言 都提供了对应的方法 来解析JSON
        6. JSON格式的字符串 转化完毕之后 会变成 数组 对象  

 注意：php中要写header('content-type:application/json;charset=utf-8');

##### JSON的写法 --

 用来表示对象*

  对象使用 { } 

  属性名 必须使用 "" 包裹

  属性值 必须使用 "" 包裹 如果属性值是数值 可以不使用双引号

~~~
var JSONObject = '{"name":"刘亦菲","skill":"失忆"}';
    console.log(JSONObject);
    // 转化为 对应的 对象(数组)
    var obj = JSON.parse(JSONObject);
    console.log(obj);
    console.log(obj.name+'|'+obj.skill);

    // JSON的写法 -- 用来表示数组 [] 中括号即可
    var JSONArr = '["绿色的花菜","大蒜","大葱","番茄","圣女果"]';
    console.log(JSONArr);
    // 转化为 对应的 数组(对象)
    var arr = JSON.parse(JSONArr);
    console.log(arr);
    console.log(arr[2]);

    // JSON的写法 -- 对象数组
    var JSONObjArr = '{"name":"彭林","skill":"约跑","runfriends":["周林林","林立群","飞哥"]}';
    console.log(JSONObjArr);
    // 转化为对应的 对象 数组
    var result = JSON.parse(JSONObjArr);
    console.log(result);
    console.log(result.runfriends[1]);
~~~

 *// 错误 总结*

  *// JSON的载体是 ==> 字符串*

  var JSONString = '{"name":"jack"}';

  *// 属性名 属性值 必须使用 双引号包裹*

  var JSONString2 = "{\"name\":\"jack\"}";

  *// 对象 键值对 之间使用 , ;*

##### ajax解析json例子

1. 浏览器端
   发送ajax请求
2. 服务器端
   读取 
   并返回 JSON格式的 字符串
3. 浏览器端
   回调函数中
      JSON的载体是 字符串
      xhr.responseText
      JSON.parse(xhr.responseText);

~~~
json：
[{
    "name": "吴京",
    "skill": "徒手抓狼"
  },
  {
    "name": "吴彦祖",
    "skill": "帅气"
  },
  {
    "name": "张国荣",
    "skill": "霸王别姬"
  },
  {
    "name": "林俊杰",
    "skill": "小酒窝--美美哒"
  }
]

php:
<?php
  // JSON也要设置一段内容 (可选)
  // 告诉浏览器 返回的是 JSON格式的数据 编码是 utf-8
  header('content-type:application/json;charset=utf-8');

  // 读取JSON文件
  $jsonString = file_get_contents('data/stars.json');

  // 返回读取的内容
  echo $jsonString;
?>


html:
<!DOCTYPE html>
<html lang="zh-CN">

<head>
  <meta charset="UTF-8">
  <title>title</title>
</head>

<body>
  <input type="button" value='获取JSON格式的数据'>
</body>

</html>
<script>
  // 点击时候获取
  document.querySelector('input').onclick = function () {
    //1.创建异步对象
    var xhr = new XMLHttpRequest();
    //2.设置请求行
    xhr.open('post', 'backJSON.php');
    //3.设置请求头(get请求可以省略)
    //4.注册状态改变事件
    xhr.onreadystatechange = function () {
      //4.1判断状态&请求是否成功并使用数据
      if (xhr.readyState == 4 && xhr.status == 200) {
        // JSON的载体是字符串 用responseText 即可获取
        console.log(xhr.responseText);
        // 转化为 对应的 对象(数组)
        var arr = JSON.parse(xhr.responseText);
        console.log(arr);
        // 遍历打印
        for(var i =0;i<arr.length;i++){
          var currentObj = arr[i];
          console.log('姓名:'+currentObj.name+'  技能:'+currentObj.skill);
        }
      }
    }
    //5.发送请求
    xhr.send(null);
  }
</script>
~~~

##### 怎么找到别的网站上的json数据--京东例子

1. 找到使用ajax的 网站
   基本上都有
2. 如何查看网站使用的ajax请求
   开发者界面
     network 选到 XHR 选项
3. 找到数据之后 如何复制过来自己玩

