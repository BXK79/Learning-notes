###                                                                                                         PHP篇

#### 了解PHP

 ~~~
<?php   写入这里的都属于PHP
>
 ~~~

##### echo输出内容

~~~
<?php
  // 这是单行注释
  /*
     这是多行注释
  */

  // 设置页面编码格式 使用php的函数
  // php是最好的语言  基本上我们能够想到的功能 都有函数 4000多个
  header('content-type:text/html;charset=utf-8');

  // 输出内容 关键字 
  echo 'hello world';

  // 中文
  echo '你好 世界';

?>
~~~

一些基本逻辑结构示例：比如while for循环、条件选择等

##### if

##### for

##### switch

##### while

##### do while

~~~
<?php
  // 设置编码格式
  header('content-type:text/html;charset=utf-8');

  // 定义变量 var name = 'jack';
  // 字符串
  $name = '吴京';

  echo $name;

  // 输出换行
  echo '<br>';
  
  // 数组
  $num = 123;
  echo $num;

    // 输出换行
  echo '<br>';

  // 小数
  $pi = 3.141592653;
  echo $pi;

  // 输出换行
  echo '<br>';
  // bool
  $male = false;

  // 逻辑语句
  if($male == false){
    echo '萌妹子';
  }else{
    echo '糙汉子';
  }
?>
~~~

~~~
<?php
  // 设置中文编码
  header('content-type:text/html;charset=utf-8');

  // 选择语句
  $day = '礼拜八';

  // 选择语句
  switch($day){
    case '礼拜一':
    case '礼拜二':
    case '礼拜三':
    case '礼拜四':
    case '礼拜五':
    echo '上班';
    break;
    case '礼拜六':
    echo '加班';
    break;
    case '礼拜天':
    echo '继续加班';
    break;
    default:
    echo '终于可以休息了';
    break;
  }

  // 循环语句
  // for
  // for(var i =0;i<10;i++){}

  for($i=0;$i<10;$i++){
    echo '<br>';
    // php中拼接字符串 用 .
    echo '感觉自己萌萌哒'.$i;
  }

  // while
  $num = 0;
  while($num<=998){
    echo '哈哈哈哈哈'.$num;
    echo '<br>';
    $num++;
  }
  // 如果为false 一次都不执行
  while(false){
    echo '人呢?';
  }
  // do while 最起码执行一次 哪怕是false
  do {
   echo '进来了吗?';
  } while (false);
?>
~~~

##### 数组

~~~
<?php
  // 设置中文编码
  header('content-type:text/html;charset=utf-8');

  // 定义数组 -- 索引数组
  $foodArr = array('榴莲','西兰花','鸡蛋','西兰花炒蛋');

  // 获取数组的元素
  // 下标从0开始
  echo $foodArr[3];

  echo '<br>';

  // 直接打印 数组的 所有内容
  // echo $foodArr;
  // 直接输出复杂类型
  print_r($foodArr);

  echo '<br>';
  // 获取数组的长度
  // 参数1 需要获取长度的数组
  echo count($foodArr);

  // 遍历 没有.length的语法
  // 获取数组的长度
  for($i=0;$i<count($foodArr);$i++){
      echo '<br>';
      echo $foodArr[$i];
  }

?>
~~~

##### 索引数组 关系型数组

~~~
<?php
  // 设置中文编码
  header('content-type:text/html;charset=utf-8');

 // 关系型数组
 $person = array('name'=>'吴京','film'=>'战狼','wife'=>'谢楠');

 // 获取 内容
 echo $person['wife'];

 // 完整输出数组
 print_r($person);

 echo '<br>';

 // 遍历
 // $key 循环的键
 // $value 循环的值
 foreach ($person as $key => $value) {
   // 打印内容
   echo $key.'-----'.$value.'<br>';
 }

?>
~~~

##### 二维数组

~~~
<style>
  span{
    color:hotpink;
    font-size:100px;
  }
</style>
<?php
  // 设置中文编码
  header('content-type:text/html;charset=utf-8');

  // 二维数组
  $starArr = array(
    array('name'=>'刘德华','film'=>'无间道','friend'=>'曾志伟'),
    array('name'=>'吴京','film'=>'战狼2','friend'=>'张翰'),
    array('name'=>'黄渤','film'=>'疯狂的石头','friend'=>'林志玲'),
    array('name'=>'汪峰','film'=>'春天里','friend'=>'那英')
  );

  // echo $starArr[2]['film'];

  for($i=0;$i<count($starArr);$i++){
    echo '<h2>明星:<span >'.$starArr[$i]['name'].'</span>出演了:'.$starArr[$i]['film'].'好朋友是:'.$starArr[$i]['friend'].' <br></h2>';
  }
?>
<p>写在php标签之外的代码  不会执行 原封不动的返回给浏览器</p>
<a href="#">戳我有惊喜哦</a>
~~~

##### 生成页面步骤（解析数据）

~~~
      生成页面的步骤
        1.商业的网站的数据 是保存在?=>服务器的数据库中
          这里使用了一个 数组 作为模拟 =>假数据
        2.用户访问这个页面时
          读取数据
        3.生成对应的html代码 =>让页面好看一些
    -->

  <?php
    // 1.引入其他的php页面
    include 'data_fruit_list.php';

    // 2.读取数据
    //$arr = array();
$arr[0] = array('href' => 'detail/detail1.php?flag=banana','path' => 'img/banana1.jpg','name' => '香蕉');
    echo '<ul>';
    for($i=0;$i<count($arr);$i++){
      // 3.生成对应的html结构
      echo '<li>';
      echo '<a href="'.$arr[$i]['href'].'">点我看'.$arr[$i]['name'].'</a>';
      echo '<img src="'.$arr[$i]['path'].'" alt="">';
      echo '<span>'.$arr[$i]['name'].'</span>';
      echo '</li>';
    }
    echo '</ul>';
     
  ?>
~~~

##### form表单发送数据（）

1. 列表页
   index.php
      数据库获取数据==>假数据
      解析数据
      生成页面
   点击a标签
      跳转  具体的url 存在的页面 lol_detail.php
      能够提交数据  lol_detail.php?name=
2. 详情页
   接收提交的数据
   去数据库查询=>假数据模拟
   生成html页面
   返回给用户

~~~
<!DOCTYPE html>
<html lang="zh-CN">

<head>
  <meta charset="UTF-8">
  <title>title</title>
</head>

<body>
  <!--  form表单发送数据
        action 指定提交的 url
        提交的方式  method
                get 不设置 默认是 get
                post
        表单元素 需要提交的数据
          使用name属性 进行标记
            name属性的值 可以随便起
            但是尽可能起的有意义
  -->
  <form action="./getData.php" method="get">
    <!--  文本框  -->
    <input name='superStarName' type="text" placeholder="请输入你喜欢的明星">
     <!--  提交按钮 -->
    <input type="submit">
  </form>
</body>

</html>
~~~

~~~
<?php 
    // echo '123';
    // 设置中文编码
    header('content-type:text/html;charset=utf-8');

    // 接收提交过来的数据
    // get
    // php中 为我们提供了一些 超全局变量
    // 跟 js中 window对象类似 不需要定义直接就可以使用
    // 是有值的
    echo $_GET;
    echo '<br>';
    print_r($_GET);
    // post 
 ?>
~~~

##### 查询LOL英雄案例

  action:提交的 url
        method:
              默认就是get 
        form表单通过get提交数据的本质是
            在url的后面 拼接上 name = value
            格式 lol_detail.php?lolHero=提莫
              在url的后面?key=value
            如果自己能够拼接url 也能够实现数据的提交

~~~
（浏览器中）
<!DOCTYPE html>
<html lang="zh-CN">

<head>
  <meta charset="UTF-8">
  <title>title</title>
</head>

<body>
  <!--
        action:提交的 url
        method:
              默认就是get 
        form表单通过get提交数据的本质是
            在url的后面 拼接上 name = value
            格式 lol_detail.php?lolHero=提莫
              在url的后面?key=value
            如果自己能够拼接url 也能够实现数据的提交
    -->
  <h2>英雄查询页面</h2>
  <form action="./lol_detail.php">
    <!--输入框  -->
    <input name='lolHero' type="text" placeholder="请输入你喜欢的英雄">
    <!-- 提交按钮  -->
    <input type="submit">
  </form>
</body>

</html>
~~~

~~~
（服务器中）
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>title</title>
</head>
<body>
<?php 
    // 设置中文编码
    header('content-type:text/html;charset=utf-8');
    // 接收数据 超全局变量
    // 获取提交过来的英雄的名字
    $heroName = $_GET['lolHero'];

    // 查询数据=>模拟的假数据中查询
    include './data_lol_detail.php';
    $hero = $heroArr[$heroName];
    
    // 生成页面 解析数据 创建好看的html结构 并返回
    echo '<h2>'.$hero['champion_title'].'--<span>'.$hero['champion_name'].'</span></h2>';
    // 图片
    echo '<img src="'.$hero['champion_icon'].'" alt="">';
    // 个性签名
    echo '<p>'.$hero['champion_info'].'</p>';
    // 位置
    echo '<h3>'.$hero['champion_tags'].'</h3>';
 ?>
</body>
</html>


~~~

##### 循环输出HTML

  当要输出的HTML结构较复杂是使用，减少字符串拼接，示例如下（了解即可）

~~~
<?php
  // 准备一些数据
  $heroArr =array(
    array(
    "champion_icon"=> "images/1498811286269.jpg",
    "champion_name"=>"凯隐",
    ),
    array(
    "champion_icon"=> "images/1493291013439.jpg",
    "champion_name"=>"洛",
    ),
    ?>
<!--  for循环的半边  -->
<?php for ($i=0; $i < count($heroArr); $i++) {  ?>
    <h1> <?php echo $heroArr[$i]['champion_name']; ?> ---- 感觉自己萌萌哒的 ^_^</h1>
    <img src="" alt="">
    <a href="#">萌你奶奶个腿</a>
<?php } ?>
~~~



语法本身没有意义，意义在于如何用于发去干一下什么事情



##### get方式提交数据

get 方式提交数据 总结
    1.数据是拼接在url中
      数据的安全性不好

 2.数据的长度问题
    理论上来说 url的长度 是可以任意修改的
    有一些浏览器会限制url的长度
    有一些服务器 对于长度太长的 url 直接就 屏蔽了
3.优点： 测试方便

多条数据的格式：
      /getData.php?food=榴莲&doType=高压锅炖
      xxx.php?key1=value1&key2=value2&key3=value3

​       print_r($_GET); //php中获取get方式提交，数组形式

  ##### post提交数据

 post提交数据
      action属性 提交的url
      必须设置 method 默认是get
    表单元素name属性
    提交按钮

post
    1.提交的数据不在url中
      安全性好一些
    2.post提交数据 没有长度限制
      浏览器端只要你想 随意添加
      服务器可以选择是否接受这么多的数据
    3.如果要上传文件 必须使用 post

##### 爱旅行案例步骤

1. 需求
   用户默认访问打开的是 登陆页面
     登陆改为index
     首页换个名字
   登陆页面 输入完毕数据之后 点击提交 跳转到了 首页
     修改登录页面的一些设置
       action
       method
       name
       submit
   跳转到了首页之后 在首页的右上角 没有 注册跟登陆 取而代之的是 
       xxx 你怎么才来
       删除默认的 注册跟 登陆
       获取通过post提交的数据 输出在 对应的位置即可

2. 前后台合作
   前端程序员 写好网站的 静态页面 -->搞定
            用户现在对于界面的要求越来越高
            主要写页面的各种各样的交互效果
   后端程序员
      根据网站的业务逻辑
        业务逻辑-> 没有登录 去登录页 没有注册 去注册页 密码输入3次失败 冻结账号 登录成功之后 去首页 获取最新的 信息展示给用户
3. get还是post
   如果是 像爱旅行 这种 跳转的方式 提交数据 一般是后端程序员来实现
   后端程序员 根据 需求 选择用哪种方式提交
4. 页面不跳转 提交数据  ajax
   发送请求给服务器  也分 get 跟post
   工作时 因为后端代码 是后台程序员来编写 

##### 上传文件（必须用post方式）

准备工作：（HTML）

​                提交方式 - post
​                  form表单
​                  input type = file name属性
​                  提交按钮
​                  form表单属性
​                  enctype 默认值是 application/x-www-form-urlencoded
​                  提交文件 multipart/form-data

​                  （PHP）

​       // 接收提交的文件

​      // 超全局变量 用来接收提交的文件
​       print_r($_FILES);

 /*
    这是 打印出来的 $_FILES的值
    Array ( 
      [icon] => Array ( 
          [name] => 1405937554667.png  上传的文件名
          [type] => image/png 类型
          [tmp_name] => C:\Users\51772\AppData\Local\Temp\php52F7.tmp 路径 临时
          [error] => 0 错误编码
          [size] => 13949  大小
          ) 
        )
  */

  // php 代码执行完毕之后 临时文件就被销毁了

  // 如果想要看到那个临时文件 可以 让php代码 执行的 稍微 慢一些  休息一会
  // sleep 的目的只是为了 让大家看到临时文件
  // sleep(5);

  // 移动上传的文件 在 w3cschool的 文件分类中
  // move_uploaded_file(file,newloc)
  // 参数1 移动的文件
  // 参数2 移动到哪里去

~~~
 move_uploaded_file($_FILES['icon']['tmp_name'],'./files/'.$_FILES['icon']['name']);
?>
~~~

<!--
    如果要严谨一些 还需要添加一些判断
      判断文件类型
      判断文件是否存在
      判断文件的大小
      ...



