### 1.初入jQuery

页面加载完后执行的方法的三种写法：

~~~javascript
window.onload=function(){
        alert("页面加载完毕")
    }
    /**
    1.$(document) $是jQuery中的名称，document是函数的参数，作用是document对象变成jQuery函数库可以使用的对象

    2.ready  是jQuery中的函数，是准备的意思，当页面的document对象加载成功后会执行ready函数的内容， 相当于js中的onload事件

    3.function（）自定义的表示onload后要执行的功能 类似于onload=function(){ alert("shi") }
     */
    //2.
    $(document).ready(function () {
        alert("页面加载完毕")
    })
    //3.
    $(function () {

    })
~~~

#### jQuery与dom的区分

###### dom对象

1. 使用JavaScript的语法创建的对象叫dom对象，也就是js对象
2. 例：var obj =document.getElementById("txt"); obj是dom对象

###### jQuery对象

1. 使用jQuery语法表示对象叫做jQuery对象
2. 注意：jQuery表示的对象是数据
3. 例如： var jobj =$("#text"), jobj就是使用jQuery语法表示的对象，也就是jQuery对象，它是一个数组，只不过现在数组中就一个值

###### dom对象可以和jQuery对象互相转换

1. dom对象转换为jQuery  **语法**：$(dom对象)
2. jQuery对象转换为dom对象  **语法**：从数组中获取第一个对象，第一个对象就是dom对象，使用[0]或者get(0)

###### dom对象和jQuery对象互相转换实例

1. dom对象转换为JQuery对象

   ~~~javascript
   <script type="text/javascript" src="../js/jquery-3.4.1.js"></script>
   <script type="text/javascript">
   function btnclick() {
   //获取dom对象
       var btn = document.getElementById("btn")
       alert("dom对象：" + btn.value)
   //把dom对象转为jquery，使用jQuery库中的函数
       var obj = $(btn)
       alert("jQuery对象：" + obj.val())
   }
   </script>
   
   
   <input id="btn" type="button" onclick="btnclick()" value="dom对象转换为jQuery对象">

2. jQuery对象转换为dom对象

   ~~~javascript
   <script type="text/javascript" src="../js/jquery-3.4.1.js"></script>
   <script type="text/javascript">
   function btnsum() {
       
   var btn = $("#btn1")[0];
   
   alert(btn.value)
   }
   </script>
   <input type="button" value="计算平方" onclick="btnsum()" id="btn1">

### 2.选择器

​		选择器：就是一个字符串用来定位DOM对象，定位了DOM对象就可以通过jQuery函数操作DOM

#### 2.1常用的选择器

1. id选择器： 语法：  $("#dom对象的id值")

   ​		通过dom对象的id定位dom对象的， 通过id找对象，id在当前页面中是唯一的

2. class选择器： 语法：  $(".class样式名"）

   class表示css中的样式，使用样式的名称定位dom对象

3. 标签选择器 ：  语法：  $("标签名称")

   使用标签名称定位dom对象的

   

#### 2.2选择器的使用

~~~javascript
<script type="text/javascript" src="../js/jquery-3.4.1.js"></script>
<script type="text/javascript">
    $(function () {
        //id选择器
        const id = $("#id")
        id.css("background","red")
        //class选择器
        const class1=$(".class")
        class1.css("background","red")
        //标签选择器
        const obj = $("div")
        obj.css("background","red")
    	function click(){
            const all = $("*")
            all.css("background","red")
        }
    })
</script>
<p id="id">id选择器</p>
<p class="class">class选择器</p>
<div>标签选择器</div>
<input type="button" value="全部选择器" onclick="click()"\>
~~~

#### 2.3混合选择器

~~~javascript
<script type="text/javascript" src="../js/jquery-3.4.1.js"></script>
<script type="text/javascript">
    $(function () {
        //id选择器
        const id = $("#id")
        id.css("background","red")
        //class选择器
        const class1=$(".class")
        class1.css("background","red")
        //标签选择器
        const obj = $("div")
        obj.css("background","red")
    //混合选择器
        const m = $("#id,.class")
        m.css("background","green")
    })
</script>
<p id="id">id选择器</p>
<p class="class">class选择器</p>
<div>标签选择器</div>
~~~

#### 2.4表单选择器

​		表单相关·元素选择器是指文本框、单选框、复选框、下拉列表等元素的选择方式。该方法无论是否存在表单<form>均可做出相应选择。表单选择器是为了更加容易地操作表单，表单选择器是根据元素类型来定义的

​	**语法：$(":type 属性值")**

例如：选择所有的单行文本框：$(":text")

~~~javascript
<script type="text/javascript" src="../js/jquery-3.4.1.js"></script>
<script type="text/javascript">
    function form1() {
        const text=$(":text")
        alert(text.val())
    }
</script>
<input type="text" value="表单选择器">
<input type="button" onclick="form1()" value="按钮">
~~~

#### 2.5 获取元素相关的对象

一、js 获取元素dom(父节点,子节点,兄弟节点)：

~~~javascript
    var test = document.getElementById("test");
　　var parent = test.parentNode; // 父节点
　　var chils = test.childNodes; // 全部子节点
　　var first = test.firstChild; // 第一个子节点
　　var last = test.lastChile; // 最后一个子节点　
　　var previous = test.previousSibling; // 上一个兄弟节点
　　var next = test.nextSbiling; // 下一个兄弟节点
~~~

二、jquery 获取元素dom(父节点,子节点,兄弟节点)：

~~~javascript
    $("#test1").parent(); // 父节点
    $("#test1").parents(); // 全部父节点
    $("#test1").parents(".mui-content");
    $("#test").children(); // 全部子节点
    $("#test").children("#test1");
    $("#test").contents(); // 返回#test里面的所有内容，包括节点和文本
    $("#test").contents("#test1");
    $("#test1").prev();  // 上一个兄弟节点
    $("#test1").prevAll(); // 之前所有兄弟节点
    $("#test1").next(); // 下一个兄弟节点
    $("#test1").nextAll(); // 之后所有兄弟节点
    $("#test1").siblings(); // 所有兄弟节点
    $("#test1").siblings("#test2");
    $("#test").find("#test1");
~~~

三、元素筛选

~~~javascript
   $("ul li").eq(1); // 选取ul li中匹配的索引顺序为1的元素(也就是第2个li元素)
    $("ul li").first(); // 选取ul li中匹配的第一个元素
    $("ul li").last(); // 选取ul li中匹配的最后一个元素
    $("ul li").slice(1, 4); // 选取第2 ~ 4个元素
    $("ul li").filter(":even"); // 选取ul li中所有奇数顺序的元素
~~~



### 3.过滤器

jQuery对象中存储的DOM对象顺序与页面标签声明位置关系

~~~
<div>1</div>
<div>2</div>
<div>3</div>
$("div")==[dom1,dom2,dom3]  //数组中的对象跟html的顺序相同
//过滤器就是过滤条件，对已经定位到数组中DOM对象进行过滤筛选，过滤条件不能独立出现在jQuery函数，如果使用只能出现在选择器后方。
~~~

**定义:**在定位了dom对象后，根据一些条件筛选dom对象，过滤器是一个字符串，用来筛选dom对象的。**过滤器不能单独使用，必须和选择器配合使用**

#### 3.1基本过滤器

##### 1.选择第一个frist，保留数组中第一个DOM对象

​	***语法：$("选择器：first")***

##### 2.选择最后个last，保留数组中最后DOM对象

​	***语法： $("选择器：last")***

##### 3.选择数组中指定对象

​	***语法： $("选择器:eq(数组索引)")***

##### 4.选择数组中小于指定索引的所有DOM对象

​	***语法： $("选择器:lt(数组索引)")***

##### 5.选择数组中大于指定索引的所有DOM对象

​	***语法： $("选择器:gt(数组索引)")***

~~~javascript
<script type="text/javascript">
    $(function () {
        //给按钮1添加点击事件
        $("#btn1").click(function () {
            $("div:first")[0].innerText="选择了第一个div"
        })
        //给按钮2添加点击事件
        $("#btn2").click(function () {
            $("div:last")[0].innerText="选择了最后个个div"
        })
        //给按钮3添加点击事件
        $("#btn3").click(function () {
            $("div:eq(3)")[0].innerText="选择了第四个div"
        })
        //给按钮4添加点击事件
        $("#btn4").click(function () {
            $("div:lt(3)").css("background","red")
        })
        //给按钮5添加点击事件
        $("#btn5").click(function () {
            $("div:gt(2)").css("background","green")
        })

    })
</script>

<div>1</div>
<div>2</div>
<div>3</div>
<div>4</div>
<div>5</div>
<input type="button" value="获取第一个div" id="btn1">
<input type="button" value="获取最后一个div" id="btn2">
<input type="button" value="获取第四个div" id="btn3">
<input type="button" value="获取小于3的div" id="btn4">
<input type="button" value="获取大于3的div" id="btn5">
~~~

##### 6.根据属性值获取不含指定属性或属性值的元素

语法：$(" 选择器[ 属性名 = ' 属性值 ']")

​			$(" 选择器[ 属性名 ！= ' 属性值 ']")

##### 7.根据属性值获取给定属性包含指定属性值的元素

语法：$(" 选择器[ 属性名 *= ' 属性值 ']")

~~~
<input name="username" /> 
<input name="userpwd" /> 
<input name="stepuser" />

$("input[name*='user']") 

~~~

##### 8.根据属性值获取以指定值开始的元素

语法：$(" 选择器[ 属性名 ^= ' 属性值 ']")

##### 9.根据属性值获取以指定值结尾的元素

语法：$(" 选择器[ 属性名 $= ' 属性值 ']")

##### 9.根据属性值获取以指定值结尾的元素

#### 3.2表单属性过滤器

​	根据表单中dom对象的状态情况，定位dom对象的。

##### 	3.3.1表单的状态：

​				启用状态：enabled

​				不可用状态：disabled

​				选择状态：checked   	例如radio checkbox

​			

##### 3.2.2选择可用的文本框

​	***语法：$(":text:enabled")***

##### 3.2.3选择不可用的文本框

​	***语法：$(":text:disabled")***

##### 3.2.4复选框选中的元素

​	***语法：$(":checkbox:cheked")***

##### 3.2.5选择指定下拉列表的被选中元素

​		***语法：$("选择器>option:selected")***

~~~javascript
<!--表单属性过滤器的使用-->
<script type="text/javascript">
    $(function () {
        //获取所有可以使用的文本框
        $("#btn01").click(function () {
           const enabledText=$(":text:enabled")
            //设置jQuery数组中所有dom对象的value值
            enabledText.val("选择了可用的文本框")
        })
        //获取不可编辑的文本框
        $("#btn02").click(function () {
            const disabledText = $(":text:disabled")
            disabledText.val("选择了不可编辑的文本框")
        })
        //获取复选框中选中了的的元素
        $("#btn03").click(function () {
            const checkboxInput  = $(":checkbox:checked")
            for (let i = 0; i <checkboxInput.length; i++) {
                // alert(checkboxInput[i].value)
                alert($(checkboxInput[i]).val())
            }
        })

        //选择指定下拉列表的被选中元素
        $("#btn04").click(function () {
            const option = $("#select>option:selected")
            alert(option.val())
        })
    })

</script>

<input type="text" value="text1"><br>
<input type="text" value="text2" disabled="true"><br>
<input type="text" value="text3"><br>
<input type="text" value="text4" disabled><br>
<input type="checkbox" value="游泳">游泳<input type="checkbox" value="唱歌">唱歌<input type="checkbox" value="跳舞">跳舞
<br>
<select name="" id="select">
    <option value="java" >java</option>
    <option value="html">html</option>
    //select属性 选择其他option选项后，其会自动设置select属性
    <option value="c++" selected>C++</option>
</select>
<br>
<button id="btn01">选择可用的文本框</button>
<button id="btn02">选择不可用的文本框</button>
<button id="btn03">复选框选中的元素</button>
<button id="btn04">选择指定下拉列表的被选中元素</button>
~~~





### 4.函数

#### 4.1 基本函数

​	对标签的值或属性进行修改或查询操作

##### 4.1.1  val

​	操作数组中DOM对象的value属性

​	***$( "选择器" ).val( )***  :  无参数调用形式。读取数组中第一个DOM对象的value属性值

​	***$( "选择器" ).val( 值 )***  :  有参形式调用；对数组中所有DOM对象的value属性值进行统一赋值

##### 4.1.2 text

​	操作数组中所有DOM对象的【文字显示内容属性】

​	***$(" 选择器 ").text()***  : 无参数调用。读取数组中所有DOM对象的文字显示内容，将得到内容拼接为一个字符串返回 

​	***$(" 选择器 ").text( 值 )***  :  有参数调用。对数组中所有DOM对象的文字显示内容进行统一赋值

##### 4.1.3 attr

​	获取Dom元素的某个属性的值或给其某个属性赋值，需要注意的是，attr函数只能获取Dom元素中，除true或false的属性的值

​	***$(" 选择器 ").attr(" 属性名 ")***  :   获取DOM数组中第一个对象的属性值

​	***$(" 选择器 ").attr(" 属性名 "，" 值 ")***  :  对数组中所有DOM对象的属性设为新值  

##### 4.1.4 prop

​	用来获取值是true/false 的属性的值。

​	语法：$(" 选择器 ").prop(" 属性名 ")

------

#### 4.2 操作函数

​		对DOM对象及其子对象进行添加或删除操作

##### 4.2.1 remove

​	**语法：*$(" 选择器 ").remove( )***   		--将数组中<u>所有DOM对象及其子对象</u>一并删除



##### 4.2.2 empty

​	**语法**：***$(" 选择器 ").empty( )***			--将数组中<u>所有DOM对象的子对象</u>删除



##### 4.2.3 append

​	**语法**：***$(" 选择器 ").append(" 标签 ")***			--为数组中所有DOM对象<u>添加子对象</u>

​	例：$("a").append("<spa> 添加了span标签</span>")



~~~javascript
<script type="text/javascript">
    $(function () {
        //运用append函数，为son添加一个span标签
        $("input:first").click(function () {
            $("#son").append("<span>用户添加了一个span标签</span>")
        })
        //应用empty函数，删除son中的所有子元素
        $("input:eq(1)").click(function () {
            $("#son").empty()
        })
        //应用remove函数，删除father及其所有的子元素
        $("input:last").click(function () {
            $("#father").remove()
        })
    })
</script>

<div id="granpa">
    <div id="father">
        <div id="son"></div>
    </div>
</div>
<input type="button" value="为son添加一个子元素"><br>
<input type="button" value="删除son的子元素"><br>
<input type="button" value="删除father及其所有子元素"><br>
~~~

##### 4.2.4 html

​	设置或返回被选元素的内容（和innerHtml效果类似）

​	语法：

​			***$(" 选择器 ").html( )***	:	无参数调用，获取DOM数组中第一个元素的内容

​			***$(" 选择器 ").html( 值 )***	:	有参数调用，用于设置DOM数组中所有元素的内容

~~~javascript
<script type="text/javascript">
    $(function () {
        //对p标签里的内容进行修改
        $("#input4").click(function () {
            $("p").html("这是<span>修改完毕后</span>的p标签")
        })
        //获取p标签中的值
        $("#input5").click(function () {
            alert($("p").html())
        })
    })
</script>
<p>这是一个p标签</p>
<input type="button" id="input4" value="修改标签中的值">
<input type="button" id="input5" value="获取标签中的值">
~~~

##### 4.2.5 each

​	each是对数组，json对象或dom数组等进行遍历，对每个元素调用一次函数。

​	语法1：***$.each( 要遍历的对象 , function( index , element ){ 处理程序 } )***

​		$相当于是java中的一个类， each 就是这个类的静态方法

​	语法2：***JQuery对象.each( function( index , element ){ 处理程序 } )***

​			JQuery对象就是一个DOM数组，对整个DOM数组中的jQuery对象进行遍历，每一个element都是一个jQUery对象

​			index：数组的下标

​			element：数组的对象

<img src="C:\Users\fengshun\AppData\Roaming\Typora\typora-user-images\image-20230713101726645.png" alt="image-20230713101726645" style="zoom:80%;" />

​								index 相当于 for循环中的i，element为dom数组中的jQuery对象，只能使用JQuery对象的属性和方法



~~~Javascript
<script type="text/javascript">
    $(function () {
        //使用第一个语法，循环JSON对象
        $("#input6").click(function () {
            var person = {name:"张三",age:18,sex:"男"}
            $.each(person,function (index,element) {
                console.log("index是key="+index+"，element是value="+element)
            })
        })
        //使用第一个语法，循环DOM数组
        $("#input7").click(function () {
            const domArray=$("li")
            $.each(domArray,function (index,element) {
                //element是DOM数组中的Jquery对象
                element.innerHTML="这是第"+index+"个"
            })
        })

        //使用第二个语法，循环DOM数组
        $("#input8").click(function () {
            //JQuery对象就是一个dom数组
            $("li").each(function (index,element) {
                element.innerHTML="这是第"+index+"个"
            })
        })
    })
</script>
<ul>
    <li></li>
    <li></li>
    <li></li>
</ul>
<input type="button" value="使用第一个语法，循环JSON对象" id="input6">
<input type="button" value="使用第一个语法，循环DOM数组" id="input7">
<input type="button" value="使用第二个语法，循环DOM数组" id="input8">
~~~

##### 操作类属性的函数

| 函数名        | 含义                                     |
| ------------- | ---------------------------------------- |
| addClass()    | 向匹配的元素添加指定的类名。             |
| attr()        | 设置或返回匹配元素的属性和值。           |
| hasClass()    | 检查匹配的元素是否拥有指定的类。         |
| html()        | 设置或返回匹配的元素集合中的 HTML 内容。 |
| removeAttr()  | 从所有匹配的元素中移除指定的属性。       |
| removeClass() | 从所有匹配的元素中删除全部或者指定的类。 |
| toggleClass() | 从匹配的元素中添加或删除一个类。         |
| val()         | 设置或返回匹配元素的值。                 |



### 5.事件绑定

#### 5.1定义元素监听事件

​	***语法： $("选择器").监听事件名（处理函数）***

说明：$("选择器") 定位dom对象，dom对象可以有多个，这些dom对象都绑定事件了

​			事件名称：就是js中事件去掉on的部分，例如：js中的单击事件onclick（），在jQUery中事件名称就是click（都是小写的）

~~~javascript
<script type="text/javascript">
    $(function () {
        //当页面dom对象加载后给对象绑定事件，因为此时button已经存在内存中创建
        $("#btn").click(function () {
            alert("点击了")
        })
    })
</script>
<input type="button" id="btn" value="按钮">
~~~

#### 5.2 on() 绑定事件

​	on（）方法在被选元素上添加事件处理程序。该方法给API带来很多便利

​	***语法：$(" 选择器 ").on( event , function)***



event:  事件一个或多个，多个之间空格分开

function : 可选。规定当事件发生时运行的函数

~~~javascript
<script type="text/javascript">
    $(function () {
        //鼠标点击div 或者 指向div div的背景颜色变为红色
        $("#div").on("click mouseover",function () {
           $(this).css("background","red")
        })
        //鼠标离开div时，背景色变为粉色
        $("#div").on("mouseleave",function () {
            $(this).css("background","pink")
        })

    })
</script>
<div id="div"></div>
~~~

**以上的用法是针对固有元素，对于动态元素来讲：**

给动态元素添加事件：

​		**语法：$(" 选择器 ").on( "事件类型" , ”子选择器“ , function)**

**注意**：选择器的元素，必须是固有元素，可以是祖先元素，子选择器是目标元素

### 6.Ajax

​	使用jQuery的函数实现Ajax请求的处理

#### 6.1 $.ajax()

​	它是jQuery中实现Ajax的核心函数，所有的方法都是在内部使用此方法

​	语法： ***$.ajax( { name : value , name : value , .... } )***

说明： 参数是json的数据，包含请求方式、数据和回调方法等

​		**async**：布尔值，表示请求是否异步处理。默认是true

​		**contentType**：发送数据到服务器时所使用的内容类型，可以不写。例如 想要请求的参数是json格式的可以写：application/json

​		**data**: 规定要发送到服务器的数据，可以是：字符串、数组、多数是json格式

​		**dataType**：期望服务器相应的数据类型。jQuery从xml、json、text、html这些中测试最可能的类型

​		**error**()：如果请求失败要运行的函数

​		**success**(resp)：当请求成功时运行的函数，其中resp是自定义的形参名

​		**type**：规定请求的类型（GET或POST），默认是GET，get和post不用区分大小写

​		**url**：规定发送请求的URL

~~~javascript
        $.ajax({
            async: true, 	//是否异步处理
            contentType: "application/json",	//发送数据到服务器时所使用的内容类型
            data: {name: "lisi", age: 12},	//向服务器发送的数据
            dataType: "json",	//期望返回的数据类型
            //请求失败执行的函数
            error: function () {
                alert("出错后执行的函数")
            }, 
            //请求成功后执行的函数
            success:function (data) {
                //data就是responseText ，是jQuery处理后的数据
                alert("请求成功后执行的函数")
            },
            type:"GET",  //请求方式
            url:"www.baidu.com"  //请求发送的url
        })

    })
~~~



#### 6.2 $.get()  

​	$.get()  方法使用HTTP GET方式从服务器加载数据

​	***语法：$.get( url , data , function(resp) , dataType )***

**url**	必需的、规定请求的地址

**data**	可选的，规定连同请求发送到服务器的数据

**function( resp )**	可选的。当请求成功时运行的函数。resp是自定义形参名。

​			参数说明：resp--包含来自请求的结果数据

#### 6.3 $.post()

​	$.post()  :使用post方式发送Ajax请求  

​	***语法：$.post( url , data , function(resp) , dataType )***

**url**	必需的、规定请求的地址

**data**	可选的，规定连同请求发送到服务器的数据

**function( resp )**	可选的。当请求成功时运行的函数。resp是自定义形参名。

​			参数说明：resp--包含来自请求的结果数据

**$.post()和$.get()  他们在内部都会调用$.ajax()**



