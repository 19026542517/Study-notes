

### 总概

```javascript
new Vue({
    //template 模板语句 不能和data一起使用
    template:'',

    //指定挂载对象 不能和$mount一起使用
    el:'',

    //数据来源，可以是对象，也可以是函数
    data:{
        属性名1:{},
        属性名2:'',
    },

    //方法 定义多个函数 用于事件处理
    methods: {
        方法名(){}
    },

    //计算属性 定义多个属性，属性中含有getter 和 setter 函数，（需要返回值）
    computed:{
        计算属性名:{
            get(){ return 返回值} ,
            set(){}
        }
    },

    //侦听属性 用于侦听data中的数据变化 含有 handler 函数 （不需要返回值） 有局部和全局
    watch:{
        data中的属性名:{
            配置项:值,
            handler(newValue,oldValue){}
        }
    },

    //过滤器 定义多个过滤器函数（需要返回值） 有局部和全局
    filters:{
        过滤器名(){}
    },

    //自定义指令 有局部和全局
    directives:{
        自定义指令名:function (element,binding) {}
    },

    //组件 定义多个组件，键值对形式
    components:{
        组件的名字:组件对象
    }
}
)
```

### 1.Vue 介绍

​	Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与[现代化的工具链](https://v2.cn.vuejs.org/v2/guide/single-file-components.html)以及各种[支持类库](https://github.com/vuejs/awesome-vue#libraries--plugins)结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

### 2.Vue初篇

#### 2.1第一个Vue程序

~~~html
<head>
    <meta charset="UTF-8">
    <title>第一个vue程序</title>
<!--    引进vue，当使用script进行vue安装之后，上下文中就注册了一个全局变量：vue-->
    <script src="../js/vue.js"></script>
</head>
<body>
<div id="app"></div>
/*
第一步：创建Vue实例
1.为什么要 new Vue() ，直接调用Vue()函数不行吗？
    不行，因为直接调用Vue（）函数，不创建实例的话会出现错误
2.关于Vue构造函数的参数：options？
    option翻译为选项
    options翻译为多个选项
    Vue框架要求这个option参数必须是一个纯粹的JS对象（JSON）：{}
    主要是通过option这个参数来给Vue实例指定多个配置项
*/
<script>
    const myVue = new Vue({
        template:'<h1>Hello Vue</h1>'
    })
    /*关于template配置项？
    *   template翻译为：模块
    *   template配置项用来指定什么？------用来指定模板语句，模板语句是一个字符串形式
    * 什么是模板语句？
    *   Vue框架自己制定了一些具有特殊含义的特殊符号
    *   Vue的模板语句是Vue框架自己搞的一套语法规则
    *   我们写Vue模板语句的时候不能乱写，要遵守Vue框架的模板语法规则
    * 模板语句可以是一个纯粹的HTML代码，也可以是Vue中的特殊规则，也可以是HTML代码+Vue的特殊规则
    * 
    * template 后面的模板语句会被vue框架进行编译，转换成浏览器能够识别的HTML代码
    * */
    myVue.$mount("#app")
    /*第二步：将vue实例挂载到id=‘app’的元素位置
    * 1.vue实例都有一个$mount()方法，这个方法的作用？
    *       就是讲Vue实例挂载到指定位置
    * 2.#app显然是iD选择器，这个语法借鉴了css选择器
    **/
</script>
</body>
</html>
~~~

#### 2.2配置项

##### 2.2.1template配置项

###### 关于template配置项：

​	1.template后面指定的是**模板语句**，但是模板语句中只能有一个**根节点**

​	2，只要data中的数据发生变化，模板语句一定会重新编译，重新渲染

​	3.注意：如果使用template配置项的话，指定挂载位置的元素会被替换掉

​	4.解决：

​				目前可以不使用template来编写模板语句。这些模板语句可以直接写道html标签中。Vue框架能够找到并编译，然后渲染

~~~html
//注意：以下代码只有Vue框架才能看懂的代码
<div id="app">
    <h1>{{h1}}</h1>
    <p>{{p}}</p>
</div>
<script>
    new Vue({
        //暂时不再使用template配置项来渲染界面
        // template:'<div><h1>{{h1}}</h1><p>{{p}}</p></div>',
        data:{
            h1:'标题',
            p:'段落'
        }
    }).$mount("#app")
</script>
~~~

​		在div中直接使用胡子语法，也可以渲染到页面，，数据直接来源于data，vue会先编译#app的div的部分，然后再渲染，这样挂载位置的元素不会被替换掉

##### 2.2.2 data选项

###### 1 data选项的介绍

​	模板语句中的数据来源于：data

​	data选项的数据类型是：		——object | Function （对象或者函数）

​	作用：data实际上是给整个vue实例提供数据来源

​	注意：如果data是对象的话，**必须是纯粹的对象**（含有零个或多个的 key/value 对）

**当data是一个函数时，函数中的返回值必须是一个JSON对象**

~~~javascript
    new Vue({
        //当data是一个jSON对象时
        data:{
            message:"Hello"
        },
        //当data是一个函数时
        data:function () {
            return {
                message:"Hello"
            }
        },
        //data 函数的简写
        data(){
            return {
                message:"HELLO"
            }
        }
    })
~~~

###### 2 data选项的使用

​		***语法：{{}}***

​				1.注意：左边两个大括号或者右边大括号之间不能有其他字符包括空格

​				2.这是vue自己搞的一套语法，别的框架看不懂的，浏览器也是不能识别的，这种语法在框架中被称为：模板语句中的插值语法				（有的人把他叫做胡子语法）

例：

~~~javascript
new Vue({
        template: '<h1>我是谁？{{name}},年龄：{{peison.age}},对象为数组：数组名={{address[1].arraysname1}}</h1>',
        data: {
            name: '张三',
            peison: {
                age: 12
            },
            address: [
                {
                    arraysname: '数组名',
                    arrayno: '数组号'
                },
                {
                    arraysname1: '数组名1',
                    arrayno1: '数组号1'
                }
            ]
        }
    }).$mount("#app")
~~~

效果图：<img src="C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230714102157690.png" alt="image-20230714102157690" style="zoom:67%;" />	

##### 2.2.3 el 配置项

​	el，实际上是element的缩写，翻译过来就是元素的意思，el配置项的作用和$mount( ) 一样，都是用来指定挂在对象的

​	作用：告诉Vue实例去接管哪个容器，el:'#app' ，表示让Vue实例去接管id=‘app’的容器

例：

~~~javascript
<div id="app">
    <h1>{{h1}}</h1>
    <p>{{p}}</p>
</div>
<script>
    /*new Vue({
        // template:'<div><h1>{{h1}}</h1><p>{{p}}</p></div>',
        data:{
            h1:'标题',
            p:'段落'
        }
    }).$mount("#app")*/
    new Vue({
        // template:'<div><h1>{{h1}}</h1><p>{{p}}</p></div>',
        data:{
            h1:'标题',
            p:'段落'
        },
        // el:'#app'
        el:document.getElementById("app")
    })
</script>
~~~

el : '#app' 和  el : document.getElementById("app")  这两种写法都可以，但是前者更简介

#### 2.3解决控制台上的提示信息和错误信息

![image-20230714112458535](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230714112458535.png)



<img src="C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230714113406329.png" alt="image-20230714113406329" style="zoom:67%;" />



##### 2.3.1 Vue.config配置项

 Vue.config是Vue 的全局配置对象，其中有个producionTip属性可以设置是否生成生产提示信息。默认值为true，如果是false则表示阻止生成提示信息

​		**使用：Vue.config.productionTip = false**

注意：虽然这样设置了属性值，但是有时候浏览器依旧会有提示信息，想要彻底根除，则需到源码中更改属性值



#### 2.4 Vue实例与容器的关系

​		一个Vue实例只能接管一个容器。一旦接管到容器之后，即便后面有相同的容器，Vue也是不会渲染它的。

​	同样一个容器只能被一个Vue实例接管。



### 3. Vue的基础用法

#### 3.1模板语法之插值语法

用于动态的渲染标签里面的值

​		***语法：{{要插入的值}}***

注意：使用该语法，必须要Vue实例指向它

##### 3.1.1data中声明的变量、函数等

~~~html
//在{{}}中可以引用多个变量，之间需要用逗号分隔开
<div id='app'>
    <h1 id="h1">{{message,sayHello()}}</h1>
</div>
<script>
        new Vue({
            el:"#app",
            data:{
                message:'houhuoaqhubb',
                sayHello:function(){console.log("函数")}
            }
        })
    </script>
~~~

##### 3.1.2 常量

在{{}}中可以直接写入常量，如果是字符串常量必须要加单引号

~~~html
<div id="app">
    <h1 id="h3">{{100}}</h1>
    <h1 id="h4">{{'huihi'}}</h1>
</div>
    <script>
        new Vue({
            el:"#app",
            data:{
                message:'houhuoaqhubb',
                sayHello:function(){console.log("函数")}
            }
        })
    </script>
~~~

##### 3.1.3 JavaScript表达式

在{{}}中可以使用javaScript 语法，例如：数字运算、字符串拼接、三元表达式、函数

~~~html
<div id="app">
    //数值运算
    <h1>{{100+200}}</h1>
    //字符串拼接
    <h1>{{'aihudi0'+"haeiuh"}}</h1>
    <h1>{{'daoi'+1}}</h1>
    //三元表达式
    <h1>{{flag ? '男':'女'}}</h1>
    //函数
    <h1>{{message.split("h").reverse().join("")}}</h1>
</div>
    <script>
        new Vue({
            el:"#app",
            data:{
                flag:true,
                message:'houhuoaqhubb',
                sayHello:function(){console.log("函数")}
            }
        })
    </script>
~~~

​							效果：<img src="C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230714150249443.png" alt="image-20230714150249443" style="zoom:50%;" />

##### 3.1.4 Vue自定义的全局变量

​	模板表达式都被放在沙盒中，只能访问[全局变量的一个白名单](https://github.com/vuejs/vue/blob/v2.6.10/src/core/instance/proxy.js#L9)，如 `Math` 和 `Date` 。

注意：如果是用户自定义的全局变量，是不可以的

​	白名单的内容：

~~~
'Infinity,undefined,NaN,isFinite,isNaN,' 
'parseFloat,parseInt,decodeURI,decodeURIComponent,encodeURI,encodeURIComponent,' 
'Math,Number,Date,Array,Object,Boolean,String,RegExp,Map,Set,JSON,Intl,'
'require' 
~~~

例：

~~~z
<div id="app">
    <h1>{{'Date函数：'+Date}}</h1>
    <h1>{{'时间戳：'+Date.now()}}</h1>
    <h1>{{'Math函数：'+Math}}</h1>
    <h1>{{'求平方'+Math.ceil(2,2)}}</h1>
</div>
    <script>
        new Vue({
            el:"#app",
            data:{
                flag:true,
                message:'houhuoaqhubb',
                sayHello:function(){console.log("函数")}
            }
        })
    </script>
~~~

​			实现：<img src="C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230714151249129.png" alt="image-20230714151249129" style="zoom:50%;" />

#### 3.2模板语法之指令语法

用于动态渲染标签的属性

##### 3.2.1 介绍

1. 作用：

2. Vue框架中的所有指令的名字都是以“v-”开始，

3. Vue框架中所有的指令都是以HTML标签的属性形式存在的

   1. 虽然指令是写在标签属性位置上，但是这个指令浏览器是无法直接看懂的，是需要Vue框架进行编译的，编译之后的内容浏览器才可以看懂
   2. 例： <span 指令是写在这里的></span >

4. 指令的语法规则：

   ​			***<html 标签  v-指令名：参数="表达式">			</html 标签>***

   ​			注意：

   ​				1.表达式就是插值语法中的{{}}里面的表达式，但是指令中的表达式位置外层不能再添加一个{{}}

   ​				2.有的指令不需要参数也不需要表达式，例如：v-once

   ​				3.有的指令不需要参数，但是需要表达式，例如：v-if='表达式'

   ​				4.有的指令既需要参数，又需要表达式，例如：v-bind:参数='表达式'
##### 3.2.2 v-once 指令

   ​		语法：***<html 标签   v-once> 		</html标签>***

   ​	作用：只渲染元素一次，随后的重新渲染，元素及其所有的子节点将被视为静态内容并跳过。这可以用于优化性能。

   ```html
   <body>
       <div id="app">
           <p>{{message}}</p>
           <p v-once>{{message}}</p>
       </div>
       <script>
           new Vue({
               el:"#app",
               data:{
                   message:'hello'
               }
           })
       </script>
   </body>
   ```

通过 Vue DevTools 组件修改message的值后，第一个p标签的值会被渲染更改，而第二个p标签的值不会被渲染更改		

![image-20230714163757462](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230714163757462.png)

##### 3.2.3 v-if 指令

​	语法：***<html 标签   v-if="表达式"> 		</html标签>***

​	作用：表达式的执行结果需要是一个布尔类型的数据：true或false

​				true：这个指令所在的标签，会被渲染到浏览器当中

​				false：这个指令所在的标签，不会被渲染到浏览器中

~~~javascript
<body>
    <div id="app">
        <p v-if="a<b">a是否小于b</p>
    </div>
    <script>
        new Vue({
            el:"#app",
            data:{
                a:1,
                b:2,
                message:'hello'
            }
        })
    </script>
~~~

##### 3.2.4 v-bind 指令

​	作用：数据绑定

​	语法格式：

​				**<html 标签   v-bind:参数="表达式"> 		</html标签>**

​				参数名可以是任意，但是最好是标签自带的属性名

​	v-bind 指令的编译原理--

​			编译前： <html 标签   v-bind:参数="表达式"> 		</html标签>

​			编译后：<html 标签  参数="表达式"> 		</html标签>    参数作为标签的属性，表达式的结果作为属性的值

​	因为v-bind 很常用，所有Vue框架对该指令提供了一种简写方式

​			***语法：<html 标签   : 参数="表达式">		</html 标签>***

~~~html
<body>
    <div id="app">

        <p v-bind:x="a">sgf</p>
<!--        这个表达式带有单引号就不是变量了，而是常量-->
<!--        <p v-bind:name="'message">这个表达式带有单引号就不是变量了，而是常量</p>-->
<!--        v-bind的简写-->
        <p :name="message">v-bind的简写</p>
<!--        让文本框的值变为动态的-->
        <input type="text" name="username" value="张三">
        <input type="text" name="username" :value="name">
<!--        让超链接的地址变为动态-->
        <a :href="url">访问</a>
    </div>
    <script>
        new Vue({
            el:"#app",
            data:{
                name:"Zhangsan",
                url:'https://www.baidu.com',
                message:'hello'
            }
        })
    </script>
</body>
~~~

![image-20230714174744900](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230714174744900.png)

##### 3.2.5 v-model 指令

​	v-model与v-bind差不多，都可以完成数据的绑定

​		语法：***<html 标签   v-model:参数="表达式"> 		</html标签>***

​	v-model与v-bind的区别与联系:

​			1.v-model与v-bind这两个指令都可以完成数据的绑定

​			2.v-bind是单向数据绑定：

​					data发生改变，视图就会发生改变

​			3.v-model是双向数据绑定

​					data发生改变，视图会发生改变，视图发生改变，data也会发生改变。

​			4.v-bind可以使用在任何html标签中。**v-model只能使用在*表单类*元素上。**

​						为什么？ 因为表单类的元素才能给用户提供交互输入的界面

​			5.通常v-model指令是使用在value属性上的。

​			6.v-bind 和 v-model 都有简写方式

​				v-bind的简写方式

​							v-bind：参数·="表达式"		简写为：			:参数="表达式"

​				v-model的简写方式：
​							v-model：value="表达式" 	简 写为：			**v-mode="表达式"**

~~~html
   <div id="app">
       
	<!--v-bind 和 v-model 的区分-->
    <input type="text" :value="name">
    <input type="text" v-model:value="name1">
    <!--v-model的简写-->
    <input type="text" v-model="name">
       
</div>
</div>
<script>
    new Vue({
        el: "#app",
        data: {
            name: "Zhangsan",
            name1:"zhangsna",
        }
    })
</script>
~~~



#### 3.3 MVVM 封层思想

​	MVVM是目前前端开发领域当中非常流行的开发思想（一种框架模式），目前前端的大部分主流框架都实现了MVVM思想，例如Vue，react

​				M：model（模型或数据）	V: View（视图）	VM： ViewModel(视图模型)，VM是MVVM中的核心部分

​	MVVM模型中，倡导了Model和View进行了分离，

​	为什么要分离？

​			如果不分离：如果数据发生任意的改动，接下来我们需要编写大篇幅的操作DOM元素的js代码

​			分离之后，出现了一个VM核心，这个VM核心把所有的脏活累活都给做了。也就是说，当Model发生改变之后，VM自动更新			View。当View发生改动之后，VM自动更新Model。我们再也不需要编写操作DOM的js代码，提高了开发效率

​													<img src="C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230714202142081.png" alt="image-20230714202142081" style="zoom:80%;" />

~~~html

<!--View-->
<div id="app">
    <input type="text" v-model="text">
    <p>{{text}}</p>
</div>
<script>
    // 整个Vue实例就是一个ViewModel 简写：VM
    new Vue({
        el:"#app",
        // data就是Model
        data:{
            text:"ZANGSAN"
        }
    })
</script>
~~~

#### 3.4 认识Vue实例

通过Vue实例可以访问哪些属性？

​	Vue实例中的属性很多，有的以  $  开始，有的以   _   开始。

​	所有以  $  开始的属性，可以看作是公开的属性，这些属性是提供程序员使用的

​	所有以  _  开始的属性，可以看作是私有的属性，这些属性是Vue框架底层使用的，一般程序员很少使用

​	通过Vue的实例对象也可以访问其原型对象上的属性，例如：vm.delete...（vm是Vue的一个实例对象）

~~~html

<div id="app">
    <input type="text" v-model="text">
    <p>{{text}}</p>
</div>
<script>
    //声明一个JSON对象
    const dataobj={
         text:"ZANGSAN"
    }
    const vm = new Vue({
        el:"#app",
        //data引用 dataobj对象
        data:dataobj
    })
console.log(vm)
</script>
~~~

观察以上代码，我们发现，为什么Vue实例可以直接使用 dataobj 对象里面的属性？-----------**数据代理机制**

Vue的数据代理机制：通过这个机制，可以直接使用Vue对象中data配置项中的属性

~~~javascript
console.log(vm.text)
//值为：ZANGSAN
~~~

想要理解数据代理机制，必须先了解 Object.defineProperty() 方法

#### 3.5  Object.defineProperty() 方法

​	作用：给对象新增属性，或者设置对象原有的属性

​	语法： Object.defineProperty( 给哪个对象新增属性 ，' 新增的这个属性的属性名 '，{ 给新增的属性设置相关的配置项（key-velue)  })

​			1. velue 配置项：给属性指定值

​			2. writable 配置项：设置该属性的值是否可以被重写

​			3. enumerable 配置项 ：true 或 false 		-----------true表示属性可以遍历，false表示属性不可以遍历

​			4. configurable  配置项： true 或 false 		-----------true表示该属性可以被删除的，false表示该属性不能被删除

​			5. getter  方法  配置项：	不需要手动调用，当**读取**属性值的时候，该方法被自动调用

​														**注意**：该方法必须有返回值，这个返回值就代表这个属性的值

​			6. setter  方法  配置项		不需要手动调用，当**修改**属性值的时候，该方法被自动调用

​														set方法有个参数，这个参数可以接收传过来的值

​			**注意**：当配置项中有set或者get方法时，velue 和 writable 配置项都不能存在

~~~javascript
<script>
    //定义一个普通对象
    const phone={
        data:new Date()
    }
    //给上面的对象添加一个color属性
    Object.defineProperty(phone,'color',{value:'红色'})
    console.log(phone.color)
</script>
~~~

![image-20230714233325391](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230714233325391.png)

#### 3.6 Vue数据代理机制

##### 	3.6.1 数据代理机制定义

​				通过访问 代理 对象的属性来间接访问目标对象的属性。数据代理机制实现需要：Object.defineProperty()方法

~~~javascript
<script>
    //目标对象
    const person = {
        name: '李四'
    }
    //代理对象
    const proxy = {}
    //如果要实现数据代理机制的话，就需要给代理对象添加一个属性name
    //注意：代理对象新增的这个属性的名字和 目标对象的属性名要一致
    Object.defineProperty(proxy,"name",{
        get:function () {
            return person.name
        },
        set:function (val) {
            person.name=val
        }
    })
</script>
~~~

ES6新特性：

​		在对象中的函数/方法   :function   是可以省略的

~~~javascript
<script>
    //目标对象
    const person = {
        name: '李四'
    }
    //代理对象
    const proxy = {}
    //如果要实现数据代理机制的话，就需要给代理对象添加一个属性name
    //注意：代理对象新增的这个属性的名字和 目标对象的属性名要一致
    Object.defineProperty(proxy,"name",{
        get() {
            return person.name
        },
        set(val) {
            person.name=val
        }
    })
</script>
~~~

##### 3.6.2 Vue数据代理机制原理

~~~html
<script>
    //目标对象
    const person = {
        name: '李四'
    }
    //代理对象
    const proxy = {}
    //给代理对象添加一个属性name
    Object.defineProperty(proxy,"name",{
        get() {
            return person.name
        },
        set(val) {
            person.name=val
        }
    })
    //Vue实例
    const vm = new Vue({
        el:"#app",
        data:person
    })
</script>
~~~

其中，vm（Vue实例）就相当于proxy ，是个代理对象，通过代理对象来访问person中的属

##### 3.6.3 Vue数据代理机制对属性名的要求

​	Vue实例不会给以 _  和  $  开头的属性名做数据代理。

​			原因：	如果允许给 _  和 $  开头的属性做数据代理的话，vm这个Vue实例上可能会出现  _Xxx 或  $Xxx属性。而这个属性名可能							会和Vue框架自身的属性名冲突	

##### 3.6.4 手写Vue框架数据代理

~~~javascript
//定义一个类
class Vue{
    //定义构造方法
    //传入的值 options 是一个简单的js对象
    //options中有一个data配置项
    constructor(options) {
        // 获取所有的属性名
        Object.keys(options.data).forEach((propertyName,index)=>{
            //获取要添加属性的属性名的第一个字符
            const firstChar = propertyName.charAt(0);
            // 判断第一个字符是否是以'_' 或者 '$' 开头，如果是，则不添加属性
            if ( firstChar!= '_' &&firstChar != '$') {
                // 为对象添加set 和 get 方法
                Object.defineProperty(this,propertyName,{
                    get() {
                        return options.data[propertyName]
                    },
                    set(val) {
                        options.data[propertyName]=val
                    }
                })
            }

        })
    }
}
~~~

### 4.核心语法

指令的语法格式：

​			<html 标签     v-指令名:参数名="表达式">		</html 标签>

​	其中 表达式 可以是 常量、js表达式、Vue 实例管理的XXX

#### 4.1 事件绑定

##### 4.1.1 基础语法

------

​			Vue中元素的事件绑定是通过**v-on**指令进行的。

​				***语法格式：<html 标签     v-on:事件名="表达式">		</html 标签>***

​			注意：在Vue中，所有事件所相关的回调函数，需要在Vue实例的配置项**method**中进行定义。

​						methods是一个对象：{}

​						在这个methods对象中可以定义多个回调函数。

​				v-on 指令的简写形式

​									***语法格式：<html 标签   @事件名="表达式">		</html 标签>***



~~~html
<div id="app">
    <!--    通过 v-on 给按钮添加一个点击事件-->
    <button v-on:click="say()">{{'普通v-on'+message}}</button>
    <!--    函数简写形式按钮-->
    <button v-on:click="walk()">{{'函数简写形式'+message}}</button>
    <!--    v-on 指令简写形式-->
    <button @click="sing()">{{'v-on简写形式'+message}}</button>
</div>
<script>
    const vm = new Vue({
        el: "#app",
        data: {
            message: '按钮'
        },
        methods: {
            //定义一个函数
            say: function () {
                alert("该函数执行了")
            },
            //函数的简写形式
            walk() {
                alert("函数的简写形式")
            },
            sing() {
                alert("v-on 简写形式")
            }
        }
    })
</script>
~~~

**绑定的回调函数，如果不需要任何参数，小括号（） 可以省略**

~~~html
    <button @click="sing">{{'不需要传递任何参数的按钮'+message}}</button>
~~~

**Vue在调用回调函数的时候，如果回调函数有形参，并且调用这个回调函数的时候没有传递任何的参数，那么Vue会自动给回调函数传递一个对象，这个对象是当前发生的事件对象。**

~~~HTML
<div id="app">

	<!--    不用对回调函数传递参数-->
    <button @click="down">{{'不需要传递任何参数的'+message}}</button>
    <br>
    <!--    对回调函数传递参数-->
    <button @click="lowe('java')">{{'需要传递任何参数的'+message}}</button>
    <br>

</div>
<script>
    const vm = new Vue({
        el: "#app",
        data: {
            message: '按钮'
        },
        methods: {
            down(event) {
                //获取这个事件
                console.log(event)		//输出一个事件对象
                //获取这个事件的对象
                console.log(event.target)	//<button>不需要传递任何参数的按钮</button>
    
                //获取这个事件对象的文本
                console.log(event.target.innerHTML);	//不需要传递任何参数的按钮按钮

            },
            lowe(event) {
                console.log(event) 	//‘java'
            }

        }
    })
</script>
~~~

##### 4.1.2   '  $event  '	占位符

------

在事件触发后，即<u>想要传递其他参数，又想要传递事件对象</u>，那么需要用到：***'   $event  '	占位符***

 	语法： 在回调函数中，专门传入一个参数，这个参数只能是 $event  不能有其他的修饰，也不用单引号括起来

注意：传入的参数的顺序，需要和定义的函数的形参顺序保持一致

~~~html
<div id="app">
    //为按钮添加点击事件
    <button @click="student('java',$event)">{{'需要传递参数并且需要event对象'+message}}</button>
</div>
<script>
    const vm = new Vue({
        el: "#app",
        data: {
            message: '按钮'
        },
        methods: {
          //定义一个函数
            student(value,event){
              console.log("传入的参数的值为："+value)		//传入的参数的值为：java
              console.log("事件对象：")		//事件对象：
                console.log(event)		//PointerEvent {isTrusted: true, pointerId: 1, width: 1, height: …} 
            }
        }
    })
</script>
~~~

##### 4.1.3 关于事件回调函数中的this

------

计数器实现：点击按钮，计数器中的数字加一

第一种方式：直接使用参数

~~~html
<div id="app">
    <!--    通过 v-on 给按钮添加一个点击事件-->
    <p>计数器:{{content}}</p>
    <button v-on:click="content++">{{message}}</button>
</div>
<script>
    const vm = new Vue({
        el: "#app",
        data: {
            message: '按钮',
            content:0
        }
    })
</script>
~~~

第二种方式，通过函数实现

~~~html
<div id="app">
    <!--    通过 v-on 给按钮添加一个点击事件-->
    <p>计数器:{{content}}</p>
    <button v-on:click="add()">{{'函数'+message}}</button>
</div>
<script>
    const vm = new Vue({
        el: "#app",
        data: {
            message: '按钮',
            content:0
        },
        methods: {
            add(){
                //vm.content++;
                this.content++;
            }
            test:()=>{
        		console(this===vm);		//false
    		}
        }
    })
</script>
~~~

在Vue实例里，methods中定义的函数体里面的  this  就是Vue实例 可以通过 this 访问data里面的属性

注意：在箭头函数中，没有this，箭头函数中的this 是从父级作用域当中继承过来的，对于当前程序来说，父级作用域是全局作用域：window

#### 4.2 methods 实现原理

------

​	methods中的方法可以通过Vue实例去直接访问。

~~~javascript
<script>
    const vm = new Vue({
        el: "#app",
        data: {
            message: '按钮',
            content:0
        },
        methods: {
            add(){
                //vm.content++;
                this.content++;
                alert("函数执行了")
            }
        }
    })
vm.add()		//函数执行了
</script>
~~~

​	**methods中的方法不会做数据代理。**



自写Vue框架，实现通过Vue实例直接访问methods中定义的方法

~~~javascript
//定义一个类
class Vue{
    //定义构造方法
    //传入的值 options 是一个简单的js对象
    //options中有一个data配置项
    constructor(options) {

        //获取所有的方法名
        Object.keys(options.methods).forEach((methodName,index)=>{
            this[methodName]=options.methods[methodName];
        })
    }
}
~~~

#### 4.3 事件修饰符

------

<u>*在Vue中，事件修饰符是可以多个联合使用的，但是需要注意顺序*</u>

​				@click.self.stop  	先.self   再  .stop

​				@click.stop.self  	先.stop再  .self

问题：在点击超链接之后，会自动访问起地址，可以手动调用事件的preventDefault 方法，可以阻止事件的默认行为

~~~html
<div id="app">
    <p>{{message}}</p>
    <a href="https://www.baidu.com" @click="s()">百度</a>
</div>
<script>
    const vm = new Vue({
        el:"#app",
        data:{message:"事件修饰符"},
        methods:{
            s(){
                alert("访问百度");
                //手动调用事件的preventDefault 方法，可以阻止事件的默认行为
                event.preventDefault();
            }
        }
    })

</script>
~~~

在Vue中可以不用手动调用事件的preventDefault 方法，而是采用事件修饰符



##### 4.3.1  " . stop "  事件修饰符

------

​	作用：停止事件冒泡，等同于  event.stopPropagation();  

​		***语法: @事件名 .stop="表达式"***

~~~html
    <!--停止事件冒泡-->
    <div @click="gradpa" class="gradpa">
        <div @click="father" class="father">
            <div @click="son" class="son">
                <input type="button" @click="show" value="按钮1（允许事件冒泡）">
                <input type="button" @click.stop="show" value="按钮2（不允许事件冒泡）">
            </div>
        </div>
    </div>
~~~



##### 4.3.2  " . prevent"  事件修饰符

------

​	作用：等同于 event.preventDefault();  阻止事件的默认行为。

​		***语法: @事件名 .prevent="表达式"***

~~~html
    <!--阻止事件的默认行为-->
    <a href="https://www.baidu.com" @click="s()">百度</a><br>
    <a href="https://www.baidu.com" @click.prevent="s()">百度2（阻止事件默认行为）</a><br><br>
~~~



##### 4.3.3  " . capture"  事件修饰符

------

添加事件监听器包括两种不同的方式：

​		1.从内到外添加。（事件冒泡模式）

​		2.从外到内添加。（事件捕获模式）

​	作用：添加事件监听器时使用事件捕获模式

​		***语法: @事件名 .capture="表达式"***

~~~html
    <div @click.capture="gradpa()">
        <!--在没有添加.capture修饰符，以下元素以及这个元素的子元素，都会默认采用冒泡排序-->
        <div @click.capture="father">

            <button @click.capture="son">添加事件监听器的时候采用事件捕获模式</button>
        </div>
    </div>
//先执行gradpa函数，再执行father函数，最后son
~~~

**注意**：想要使用事件捕获模式，所有的父级元素都需要添加   " . capture"  事件修饰符



##### 4.3.4  " . self"  事件修饰符

------

​	作用：这个事件如果是“我自己元素”上发生的事件，不是别人传递过来的事件（如冒泡事件），则执行对应的程序

​		***语法: @事件名 .self="表达式"***

~~~html
    <!--不是别人传递过来的事件，则执行对应的程序-->
    <div @click="gradpa()">
        <div @click.self="father">
            <button @click="son">不是别人传递过来的事件，则执行对应的程</button>
        </div>
    </div>
//先执行son函数，再执行gradpa函数
~~~



##### 4.3.5" . once"  事件修饰符

------

​	作用：事件只发生一次，当事件触发后，后续讲不会再触发事件

​		***语法: @事件名 .once="表达式"***

~~~html
<!--事件只发生一次-->
    <button @click="son">普通事件</button>
    <button @click.once="son">事件只发生一次</button>
~~~



##### 4.3.6  " . passive"  事件修饰符

------

​	作用：passive 翻译为顺从/不抵抗。无需等待，直接继续（立即）执行事件的默认行为

​		***语法: @事件名 .passive="表达式"***

注意：. passive 和 .prevent 修饰符是对立的。不可以共存。如果一起使用就会报错。

​		.prevent ：阻止事件默认提交

​		. passive：解除限制

~~~html
//当不给事件添加 . passive 修饰符时，滚动条会延迟移动，但是如果给事件添加该修饰符之后，函数中的循环依旧会执行，但是滚动条不会受函数的影响
<!--.passive 修饰符-->
    <div class="divList" @wheel.passive="testPassive">
        <div class="item">div1</div>
        <div class="item">div2</div>
        <div class="item">div3</div>
    </div>

</div>
<script>
    const vm = new Vue({
        el: "#app",
        data: {message: "事件修饰符"},
        methods: {
            testPassive(event){
                for (let i = 0; i < 10000; i++) {
                    console.log("test passive")
                }

                //event.preventDefault()
            }
        }
    })

</script>
~~~

<video src="C:\Users\fengshun\Desktop\自学\Vue\视屏\20230715_173243.mp4"></video>



#### 4.4按键修饰符

------

##### 4.4.1 传统的处理键盘操作：

~~~html
当用户按下enter键之后，获取文本框中的文字
<div id="app">
    <h1>{{message}}</h1>
    <input type="text" @keyup="getInfo">
</div>
<script>
    const vm = new Vue({
        el:"#app",
        data:{message:"按键修饰符"},
        methods:{
            getInfo(event){
                //当用户输入回车键的时候，获取用户输入的信息
                if (event.keyCode === 13) {  //13为 enter 键
                    console.log(event.target.value)
                }
            }
        }
    })
</script>
~~~

​	**应用**：通过按键修饰符，无需在函数中判断是按下的哪个键，直接在事件名后面添加按键修饰符即可，并且，可以为事件添加多个按键				修饰符

##### 4.4.2 **常用的按键修饰符：**

1. `.enter`
2. `.tab`
3. `.delete` (捕获“删除”和“退格”键)
4. `.esc`
5. `.space` （空格）
6. `.up`
7. `.down`
8. `.left`
9. `.right`

注意：tab 键 无法触发keyup事件，而只能触发keydown事件

~~~html
<div id="app">
    <h1>{{message}}</h1>
    enter键： <input type="text" @keyup.enter="getInfo"><br>
    tab键：<input type="text" @keydown.tab="getInfo"><br>
    delete键： <input type="text" @keyup.delete="getInfo"><br>
    esc键： <input type="text" @keyup.esc="getInfo"><br>
    space（空格） 键：<input type="text" @keyup.space="getInfo"><br>
    上：<input type="text" @keyup.up="getInfo"><br>
    下：<input type="text" @keyup.down="getInfo"><br>
    左：<input type="text" @keyup.left="getInfo"><br>
    右：<input type="text" @keyup.right="getInfo"><br>
    左和右：<input type="text" @keyup.right.left="getInfo"><br>
</div>
<script>
    const vm = new Vue({
        el: "#app",
        data: {message: "按键修饰符"},
        methods: {
            getInfo(event) {
                //当用户输入回车键的时候，获取用户输入的信息
                console.log(event.target.value)
            }
        }
    })
</script>
~~~

##### 4.4.3 获取某个键的按键修饰符

------

操作：	

​		第一步：通过event.key 获取这个键的真实名字。

​		第二步：将这个真实名字以kebab-case 的这种风格进行命名

​					例如：PageDowm  命名为	page-down 

~~~html
   <div id="app">
获取键位修饰符： <input type="text" @keyup="getkey"><br>  
   </div>
    const vm = new Vue({
        el: "#app",
        data: {message: "按键修饰符"},
        methods: {
            getkey(event){
                console.log(event.key)
            }
        }
    })
~~~

##### 4.4.4 自定义按键修饰符

------

​		通过Vue的**全局配置**对象config 来进行按键修饰符的自定义

​		语法规则：

​				***Vue.config.keyCodes.按键修饰符的名字 = 键值***

~~~javascript
//定义一个 huiche 修饰符 代替回车键
Vue.config.keyCodes.huiche=13
~~~

##### 4.4.5 系统修饰键

------

​	共有四个系统修饰键：

​		1.ctrl		

​		2.alt

​		3.shift

​		4.meta



​		注意：在 Mac 系统键盘上，meta 对应 command 键 (⌘)。在 Windows 系统键盘 meta 对应 Windows 徽标键 (⊞)。在 Sun 操作系统键盘上，meta 对应实心宝石键 (◆)。在其他特定键盘上，尤其在 MIT 和 Lisp 机器的键盘、以及其后继产品，比如 Knight 键盘、space-cadet 键盘，meta 被标记为“META”。在 Symbolics 键盘上，meta 被标记为“META”或者“Meta”。



对于keydown 事件来说：只要按下ctrl 键，keydown事件就会被触发。

对于keyup 事件来说：需要按下ctrl 键，并且加上按下组合键（任意键），然后松开组合键之后，keyup才会触发，其他三个修饰符也是如此。



组合键配合使用：

~~~html
<!--按下ctrl+i键才会触发getInfo这个函数-->
    组合键配合使用： <input type="text" @keyup.ctrl.i="getInfo"><br>
<!--想要多个键配合使用，只需要在后面继续添加即可-->
<!--例如 ctrl+a 组合键 -->
<input type="text" @keyup.ctrl.a="getInfo"><br>
~~~

​		注意：该方法只适合系统修饰键，并且只能是keyup事件。

#### 4.5 计算属性（computed 配置项）

##### 前言

根据以上内容实现，可以使用v-model指令实现，字符串同步反转

~~~html
<div id="app">
    请输入字符串：<input type="text" v-model="str">
    <p>{{message}}{{str.split("").reverse().join("")}}</p>
    <p>{{message}}{{str.split("").reverse().join("")}}</p>
    <p>{{message}}{{str.split("").reverse().join("")}}</p>
</div>
<script>
    new Vue({
        el:"#app",
        data:{
            message:"反转的字符串为：",
            str:""
        },
    })
</script>
~~~

但是这样减少了代码的复用率，如果多个标签要使用反转后的字符串，则增加了代码量

改进：

~~~html
<div id="app">
    请输入字符串：<input type="text" v-model="str">
    <p>{{message}}{{strReverse()}}</p>
    <p>{{message}}{{strReverse()}}</p>
</div>
<script>
   const vm=new Vue({
        el:"#app",
        data:{
            message:"反转的字符串为：",
            str:""
        },
        methods:{
            strReverse(){
               return  this.str.split("").reverse().join("")
            }
        }
    })
</script>
~~~

但是通过这种方式，每次都会调用 strReverse 减少了效率

##### 介绍

​	定义：使用Vue的原有属性，经过一系列的运算/计算，最终得到了一个全新的属性，叫做计算属性

​			原有属性：data中的属性就是Vue的原有属性

​			全新属性：表示生成了一个新的属性，和data中的属性无关了，新的属性有自己的属性名和属性值

​	作用：代码得到复用、代码便于维护、代码的执行效率高

​	**语法**：为Vue中添加一个配置项（computed）

~~~javascript
computed:{
    			//定义多个属性
                属性1:{
                    get(){  return   },
                    set(){      }
                }
         }
~~~

​		说明：在computed配置项中添加属性，每个属性都有get 和 set 方法，当修改计算属性的值的时候，set方法被自动调用，而对于get方法来说，在第一次访问这个属性的时候会被执行一次，页面无论引用了多少次该属性，都只执行一次（缓存），然而，当data中的属性发生变化，get方法就会重新被调用

​		**简写形式：**（<u>前提：set方法不需要的时候</u>）

~~~
computed:{
    			//定义多个属性
                属性1(){
                  return ....
                }
         }
~~~



##### 使用计算属性实现字符串反转

~~~html
<!--computed 中的属性和data中的属性一样，可以在插值语法中直接调用，或者在js代码中，使用Vue实例调用-->

<div id="app">
    请输入字符串：<input type="text" v-model="str">
    <p>{{message}}{{he}}</p>
</div>
<script>
    const vm  =new Vue({
        el:"#app",
        data:{
            message:"反转后的字符串",
            str:""
        },
        computed:{
            he:{
                //计算属性需要与data中的属性关联
                get(){
                    console.log(this=== vm)		//true
                    return this.str.split("").reverse().join("")
                },
                set(val){
                    //计算属性的值变不变，取决于属性关联的vue原始属性的值
                    //修改计算属性，实际上就是通过修改vue的原始数据来实现的
                    this.he=val.split("").reverse().join("")
                }
            },
            //简写形式
            say(){
                return this.str.split("").reverse().join("")
            }
            
        }
    })
</script>
~~~

#### 4.6 侦听属性（watch 配置项）

##### 基本侦听

1. 侦听属性的变化其实就是监视某个属性的变化。当被监视的属性一旦发生改变时，执行某段代码。

2. 监视属性变化时需要使用 watch 配置项。

**语法**：

```javascript
//侦听属性配置项
watch:{
    // 需要侦听哪个属性，就定义该属性
    str:{
        //初始化的时候，自动调用一次handler
        immediate：true,
        handler(newValue,oldValue){
            
        }
    }
}
```

注意：

1. 侦听属性可以侦听data中的属性，以及**计算属性**（即computed 配置项中的属性）
2. 侦听属性的名字应该与被侦听的名字相同
3. 在侦听属性中有一个固定的方法   **handler ( newValue , oldValue )**  
4. 当侦听的属性的值发生变化的时候，handler函数就会自动调用一次，newValue表示新值，oldValue表示旧值
5. **immediate 配置项**：默认值为false ，当设置为true时，表示初始化的时候自动调用一次handler方法
6. 在handler函数中的this 也表示Vue实例

当监视的属性是个多级对象时

~~~html
<div id="app">
    请输入字符串：<input type="text" v-model="str"><br>
    <input type="text" v-model="a.b">
</div>
<script>
    const vm = new Vue({
        el: "#app",
        data: {
            message: "值：",
            a: {
                b: 0
            }
        },
        //计算属性
        computed: {
            say() {
                return "hello"
            }
        },
        //侦听属性
        watch: {
            // 需要侦听哪个属性，就定义该属性
            'a.b': {
                handler(newValue, oldValue) {
                    console.log("改变了")
                }
            }
        }

    })
</script>
~~~

###### immediate 配置项

默认值为false ，当设置为true时，表示初始化的时候自动调用一次handler方法



##### 深度监视——deep配置项

当需要监视一个具有多级结构的属性，并且监视所有的属性，需要启用深度监视

侦听属性的属性名只需要是被侦听的属性的父级属性名就可以，在侦听属性中添加**deep配置项**

​	**注意**：deep配置项默认值为false，当设置为true时，表示侦听a属性里面的所有属性

~~~html
<div id="app">
    请输入字符串：<input type="text" v-model="str"><br>
    <input type="text" v-model="a.c.d.e">
</div>
<script>
    const vm = new Vue({
        el: "#app",
        data: {
            message: "值：",
            str: "",
            a: {
                b: 0,
                c:{
                    d:{
                        e:0
                    }
                }
            }
        },
        //计算属性
        computed: {
            say() {
                return "hello"
            }
        },
        //侦听属性
        watch: {
     
            a: {
                //启用深度监视，默认值为false
                deep:true,
                handler(newValue, oldValue) {
                    console.log(" 改变了")
                }
            }
        }

    })
</script>
~~~

##### 后期添加监视

​	语法：***vm.$watch( ' 被监视的属性 ', { } )***

~~~javascript
    vm.$wacth("number",{
        handler(newValue, oldValue){
            console.log(newValue,oldValue)
        }
    })
~~~

##### handler的简写形式

当侦听属性中只有handler回调函数的时候，没有其他配置项，可以使用handler简写形式

~~~javascript
    const vm = new Vue({
        el: "#app",
        data: {
           number:0,
            str:""
        },
        //侦听属性
        watch: {
            number(newValue, oldValue){
                console.log(newValue,oldValue)
            }
        }
    })

~~~

后期添加监听属性的简写形式：

~~~javascript
    vm.$wacth("number", function(newValue, oldValue){
            console.log(newValue,oldValue)
        
    })
~~~

##### computed 和 watch 的区别

​	如果computed和watch 都能完成某个功能的时候，优先使用computed

​	如果在程序当中采用异步的形式，只能使用watch

注意：计算属性的属性名不能和data中的属性名相同，但是侦听属性的属性名就必须和data中的属性名相同

~~~javascript
//如果要添加定时器，在computed中必须要有返回值，而在watch中，可以没有返回值           

num1(newValue){
    //这里虽然是普通箭头函数，但是这不是VUe管理的，是javascript管理的
                setTimeout(()=>{
                    let resule = newValue-this.num1
                    if (resule<0){
                        this.compareResult=newValue+"<"+this.num1
                    }else if(resule>0){
                        this.compareResult=newValue+">"+this.num1
                    }else{
                        this.compareResult=newValue+"="+this.num1
                    }
                },1000*3)
            }
//针对以上的this 为什么依旧是Vue实例?
	//因为箭头函数没有this，只能向上一级找this，上一级是num1，num1是Vue实例的属性，所有this是Vue实例


			//这里虽然是普通函数，但是这不是VUe管理的，是window管理的，this也不再代表Vue实例
                setTimeout(function(){
                    let resule = newValue-this.num1
                    if (resule<0){
                        this.compareResult=newValue+"<"+this.num1
                    }else if(resule>0){
                        this.compareResult=newValue+">"+this.num1
                    }else{
                        this.compareResult=newValue+"="+this.num1
                    }
                },1000*3)

什么时候使用箭头函数呢？
	如果函数需要被Vue管理就需要使用箭头函数，如果不是Vue管理，那就不用箭头函数
~~~

#### 4.7**class** **与** **style** **绑定**

##### 前言

​	操作元素的 class 列表和内联样式是数据绑定的一个常见需求。因为它们都是 attribute，所以我们可以用 `v-bind` 处理它们：只需要通过表达式计算出字符串结果即可。不过，字符串拼接麻烦且易错。因此，在将 `v-bind` 用于 `class` 和 `style` 时，Vue.js 做了专门的增强。表达式结果的类型除了字符串之外，还可以是对象或数组。

##### 动态绑定之字符串形式

​	**使用场景**：如果动态绑定的样式个数只有一个时，可以使用字符串形式

利用之前学习的知识，实现动态更改元素样式

~~~html
 <script src="../../js/vue.js"></script>
    <style type="text/css">
        .static{
            border: 1px solid black;
            background-color: aqua;
        }
        .big{
            width: 200px;
            height: 200px;
        }
        .small{
            width: 100px;
            height: 100px;
        }
    </style>
</head>
<body>
<div id="app">
    <span>静态形式：{{msg}}</span>
    <div class="static small">{{msg}}</div>
    <br><br>

    <span>动静结合形式：{{msg}}</span>
    <div class="static" :class="l">{{msg}}</div>
    <button @click="small()">变小</button>
    <button @click="big()">变大</button>
</div>
<script>
    const vm=new Vue({
        el:"#app",
        data:{
            msg:"传统更改class",
            l:"small"
        },
        methods:{
            small(){
                this.l='small'
            },
            big(){
                this.l='big'
            }
        }
    })

</script>
~~~

<video src="C:\Users\fengshun\Desktop\自学\Vue\视屏\20230716-0303-32.9504816.mp4"></video>

##### class绑定之数组形式

​	将data中的属性定义为数组，可以添加事件来操作该数组。

~~~html
    <span>动静结合形式：{{msg}}</span>
    <div class="static" :class="classArray">{{msg}}</div>
    <button @click="background()">背景</button>
    <button @click="text()">字体</button>
</div>
<script>
    const vm=new Vue({
        el:"#app",
        data:{
            msg:"class绑定之数组形式",
            c1:"text-dange",
            c2:"active",
            classArray:[]
        },
        methods:{
            background(){
                this.classArray.push("active")
            },
            text(){
                this.classArray.push("text-dange")
            }
            
        }
    })

</script>
~~~

##### class绑定之对象形式

​	使用场景：样式的个数是固定的，样式的名字也是固定的，但是需要动态的决定样式用还是不用。

注意：对象中的属性必须要和css中定义的名字相同，并且属性的值必须是boolean类型的

~~~html
        <!--规定样式-->
<style type="text/css">
        .static{
            border: 1px solid black;
            width: 200px;
            height: 200px;
        }
        .active{
            background-color: antiquewhite;
        }
        .text-dange{
            color: aqua;
        }
    </style>

	<div class="static" :class="classobj">{{msg}}</div>

    <script>
    const vm=new Vue({
        el:"#app",
        data:{
            msg:"class绑定之数组形式",
            //属性名必须和class相同
            classobj:{	
                active:false,
                'text-dange':true
            }
        }
    })

</script>
~~~

##### style绑定

注意：样式名需要更改

​	例如：background-Color 需要更改为： backgroundColor

~~~html
<div id="app">
    <span>静态写法：{{msg}}</span>
    <div class="static" style="background-color: aqua">{{msg}}</div>
    <br>
    <span>静态写法（字符串形式）：{{msg}}</span>
    <div class="static" :style="myStyle">{{msg}}</div>
    <br>
    <span>动态写法（对象形式）：{{msg}}</span>
    <div class="static" :style="{backgroundColor: 'aqua'}">{{msg}}</div>
    <br>
    <div class="static" :style="styleobj">{{msg}}</div>
    <br>
    <span>动态写法（数组形式）：{{msg}}</span>
    <div class="static" :style="styleArray">{{msg}}</div>
</div>
<script>
    const vm=new Vue({
        el:"#app",
        data:{
            msg:"style绑定",
            myStyle:'background-color: aqua;',
            //对象
            styleobj:{
                backgroundColor: 'aqua',
                margin:'10px',
            },
            //数组
            styleArray:[
                {backgroundColor: 'aqua'},
                {margin:'10px'}
            ]
        }
    })

</script>
~~~

#### 4.8 条件渲染

v-if指令的值：true/false

​	 true：表示该元素会被渲染到页面上

​	false：表示该元素不会被渲染到页面上（不是修改了css样式，而是页面不会加载该元素）

##### v-if

~~~html
<div id="app">
    <h1>{{message}}</h1>
    <h1 v-if="true">{{message}}</h1>
    <h1 v-if="false">{{message}}</h1> <!--该元素不会被加载到页面-->
</div>
~~~

##### 在< template>元素上使用v-if条件渲染分组

​		因为 `v-if` 是一个指令，所以必须将它添加到一个元素上。但是如果想切换多个元素呢？此时可以把一个 `<template>` 元素当做不可见的包裹元素，并在上面使用 `v-if`。最终的渲染结果将不包含 `<template>` 元素。



注意：template 标签只是起到占位的作用，不会真正的出现在页面上，也不会影响页面的结构

~~~html

<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
~~~



##### v-else

也可以用 `v-else` 添加一个“else 块”：

~~~html
    <h1 v-if="false">{{message}}</h1>
    <h1 v-else>else：{{message}}</h1><!--只呈现该元素-->
~~~

注意：v-if 和 v-else 之间不能插入其他元素，否则会报错



##### v-else-if

~~~html
    <h1 v-if="number>10">number大于10</h1>
    <h1 v-else-if="number<10&&number>0">number大于0小于10</h1>
    <h1 v-else>number小于0</h1>
//number=9
~~~



##### v-show

v-show 指令是通过修改css样式中的display属性来达到显示效果，页面会加载元素

~~~html
    <h1 v-show="true">v-show</h1>
    <h1 v-show="false">v-show</h1>
~~~

v-if和v-show 应该如何选择：

​	1.如果一个元素在页面上被频繁的隐藏和显示，建议使用v-show，因为使用v-if开销比较大

​	2.v-if 的优点：页面加载速度快，提高了页面的渲染效率

#### 4.9 列表渲染

##### 列表渲染之数组

###### v-for

v-for 需要写到循环项上

语法规则：

​			v-for='  (value,index)    in/of     数组 '

~~~html
    <ul>
        <!--name为数组-->
        <li v-for="(name,index) in names ">{{name+'='+index}}</li>
    </ul>
~~~

数组中允许为多个对象：

~~~html
<div id="app">
    <h1>动态列表(数组中包含对象)：{{message}}</h1>
    <ul>
        <li v-for="(value,index) in nameObj ">{{'id为：'+value.id+'姓名为：'+value.name}}</li>
    </ul>
</div>
<script>
    const vm = new Vue({
        el:"#app",
        data:{
            message:'列表渲染',
            nameObj:[
                {id: '111',name:'张三'},
                {id: '222',name:'李四'},
                {id: '333',name:'王五'}
            ]

        }
    })
</script>
~~~





##### 列表渲染之对象

value表示对象中属性的值，name表示对象的属性名，index表示索引号

~~~html
<h1>动态列表(遍历对象)：{{message}}</h1>
<ul id='app'>
    <li v-for="(value,name,index) in person ">{{index+'='name+'='+value}}</li>	<!--例如：0=name=张三 -->
</ul>
<script>
    const vm = new Vue({
        el:"#app",
        data:{
            message:'列表渲染',
            person:{
                name:'张三',
                sex:'男',
                age:18
            }
        }
    })
</script>
~~~

##### 列表渲染之字符串

value表示字符串中的每一个字符。index为该字符的索引号

~~~html
    <h1>动态列表(遍历字符串)：{{message}}</h1>
    <ul>
        <li v-for="(value,index) in message ">{{index+'='+value}}</li>
    </ul>
<script>
    const vm = new Vue({
        el:"#app",
        data:{
            message:'列表渲染',
            person:{
                name:'张三',
                sex:'男',
                age:18
            }
        }
    })
</script>
~~~

![image-20230716161223685](C:\Users\fengshun\AppData\Roaming\Typora\typora-user-images\image-20230716161223685.png)

##### 指定次数

~~~html
    <ul>
        <li v-for="(value,index) in number ">{{index+'='+value}}</li>  //number为10
    </ul>
~~~

类似于 for（int i=0; i<number ;i++)  其中 value就是每次遍历的结果（1，2，3...） index 就是其索引

##### 虚拟dom与diff算法

Vue框架是采用了虚拟Dom机制+diff算法，来提高执行效率。

​	**虚拟DOM**：在内存当中的DOM对象。

​	**diff算法**：这是一种能够快速比较出两个事物不同之处的算法。

![image-20230716222645292](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230716222645292.png)

​	根据上图：Vue框架会从内存中读取数据，然后生成虚拟的DOM （即实例的HTML元素），最后再渲染到页面，当！内存中的数据发生改变，Vue就会从内存中读取新的数据，生成一个新的虚拟DOM，然后，通过diff算法，将新的虚拟DOM与旧的DOM对比，依此对比虚拟的DOM中的元素以及属性是否相同，如果相同，则不在重新渲染，直接生成DOM，如果遇到不相同的DOM，才渲染新的DOM到页面

####  4.10 :key 指令

在v-for指令所在的标签中，还有一个非常重要的属性，就是这个**:key** 指令

如果没有指定 ：key 属性，系统会自动将index（索引）作为key，这个key是这个dom元素的唯一标识

​	问题：当对不指定key的值的时候，key的值会默认是index，当在数组中添加元素，并且在数组首部添加元素的时候，key的值也会随之发生改变，并且生成的新的DOM元素会发生巨大的变化，导致渲染的数量巨大

解决：不建议使用index作为key的值，而是讲数组中对象里面的唯一标识（id）作为key的值

#### 4.11列表过滤

需求：在文本框中，输入某个字，列表就会呈现含义该字的所有数据

1.通过侦听属性实现

~~~html
<div id="app">
    <br>
    <h1>{{message}}</h1>
    监视属性形式： <input type="text" placeholder="请输入搜索的关键字" v-model="keyword">
    <br>
<table border="1px solid black">
    <thead>
    <tr>
        <td>选择</td>
        <td>姓名</td>
        <td>职业</td>
        <td>攻击力</td>
    </tr>
    </thead>
    <tbody>
    <tr v-for="(value,index) in filterObjects" :key="value.id">
        <td><input type="checkbox" :value="index"></td>
        <td>{{value.name}}</td>
        <td>{{value.position}}</td>
        <td>{{value.power}}</td>
    </tr>
    </tbody>

</table>
</div>
<!--核心代码：-->
<script>
    const vm = new Vue({
        el:"#app",
        data:{
            message:'列表过滤',
            objects:[{id:'0',name:'王昭君',position:'法师',power:'100'},
                {id:'1',name:'达摩',position:'战士',power:'200'},
                {id:'2',name:'后裔',position:'射手',power:'200'}],
            keyword:'',
            //定义一个空的数组，用于存放满足条件的对象
            filterObjects:[]
        },
        watch:{
            keyword:{
                //页面刷新时，自动调用一次handler函数
                immediate:true,
                handler(newValue){
                    //执行过滤规则
                    //将过滤后的数据赋值给 filterObjects 再通过v-for进行遍历渲染到页面
                    this.filterObjects=this.objects.filter((object)=>{
                        return object.name.indexOf(newValue)>=0
                    })
                }
            }
        }
    })
</script>
~~~

2.通过计算属性实现

通过计算属性 filterObjects 的方法，直接返回符合条件的数组，在v-for中直接使用 filterObjects 属性即可

~~~html
<div id="app">
    <br>
    <h1>{{message}}</h1>
    计算属性形式： <input type="text" placeholder="请输入搜索的关键字" v-model="keyword">
    <br>
<table border="1px solid black">
    <thead>
    <tr>
        <td>选择</td>
        <td>姓名</td>
        <td>职业</td>
        <td>攻击力</td>
    </tr>
    </thead>
    <tbody>
    <tr v-for="(value,index) in filterObjects" :key="value.id">
        <td><input type="checkbox" :value="index"></td>
        <td>{{value.name}}</td>
        <td>{{value.position}}</td>
        <td>{{value.power}}</td>
    </tr>
    </tbody>

</table>
</div>
<!--核心代码-->
<script>
    const vm = new Vue({
        el:"#app",
        data:{
            message:'列表过滤',
            objects:[{id:'0',name:'王昭君',position:'法师',power:'100'},
                {id:'1',name:'达摩',position:'战士',power:'200'},
                {id:'2',name:'后裔',position:'射手',power:'200'}],
            keyword:'',
        },
        computed:{
            filterObjects(){
                return this.objects.filter((object)=> {
                    return object.name.indexOf(this.keyword) >= 0
                })
            }
        }
    })
</script>
~~~

#### 4.12列表排序

​	添加三个按钮，分别为升序、降序、原序，为其添加点击事件，点击升序按钮时，修改type的值为1，点击降序按钮时，修改type的值为-1，点击原序按钮时，修改type的值为0，通过计算属性，判断type的值，来对数组排序

**注意**：数组的sort方法，会改变原有的数组，所以在进行排序时，需要讲原数组的值传递给一个新数组，在对新数组进行排序操作

~~~javascript

    const vm = new Vue({
        el:"#app",
        data:{
            message:'列表过滤',
            objects:[{id:'0',name:'王昭君',position:'法师',power:'100'},
                {id:'1',name:'达摩',position:'战士',power:'200'},
                {id:'2',name:'后裔',position:'射手',power:'300'}],
            keyword:'',
            type:0
        },
        computed:{
            filterObjects(){
                //
                const arr = this.objects.filter((object) => {
                    return object.name.indexOf(this.keyword) >= 0
                })
                if (this.type===-1){
                    //降序
                    arr.sort((o1,o2)=>{
                        return o2.power - o1.power})
                }else if (this.type===1){
                    //升序
                    arr.sort((o1,o2)=>{
                        return o1.power - o2.power
                    })
                }
                return arr
            }
        }
    })
~~~

#### 4.13表单数据收集

##### 获取数字—number修饰符

作用：在表单中，将用户输入的数字，自动转换为int类型，并且如果用户输入的字符串里有字母，它会忽略字母，只把数字转换为int类型

例如：

​		用户输入 123 	没有进行转换之前：‘123’ 		进行转换后：123

​		用户输入123ad		没有进行转换之前：‘123ad’		进行转换后：123

​	语法：

​				***<html 标签   v-model.number:参数="表达式"> 		</html标签>***

~~~
        年龄：<input type="number" v-model.number="age">
~~~

##### 获取用户名—trim 修饰符

作用：用户在单行文本框中输入任意字符，它会自动取出字符串头部和尾部的空格

​		语法：***<html 标签   v-model.trim:参数="表达式"> 		</html标签>***

​			例：用户输入 '   abd    '	,	转换后为  	'abd'

~~~html
用户名： <input type="text" v-model.trim="uname"><br>
~~~

##### 获取单选按钮

需要给单选按钮添加相同的name属性，并且指定Value的值，最后通过v-model来获取Value的值

~~~html
        <input type="radio" name="gander" value="1" v-model="gender">男
        <input type="radio" name="gander" value="2" v-model="gender">女
~~~

##### 获取多选按钮

对于checkbox 来说，如果没有手动指定Value ，那么会拿这个标签的checked属性的值（true或false）作为value

~~~html
        爱好：
        <input type="checkbox" v-model="interest" value="travel">旅游
        <input type="checkbox" v-model="interest" value="sport">运动
        <input type="checkbox" v-model="interest" value="sing">唱歌
~~~

注意：必须指定Value的值，并且v-model中的值必须为数组，即interest必须是数组。

##### 获取下拉列表

~~~html
        学历： <select v-model="grade">
        <option value="">请选择学历</option>
        <option value="zk">专科</option>
        <option value="bk">本科</option>
        <option value="ss">硕士</option>
    </select>
~~~

##### 获取文本域中的文字—lazy修饰符

使用v-model双向绑定，用户输入文字的同时，会实时获取文本域的value值，就会不断的执行底层代码，为了提高程序效率可以使用 lazy修饰符

~~~html
        <textarea name="" cols="50" rows="15" v-model.lazy="introduce"></textarea>
~~~

作用：当用户选中文本域的时候，Vue不会获取其文本域的值，而是等到失去焦点后，再获取文本域的值

##### 阻止表单默认提交

方式一：给按钮添加一个点击事件，在函数中获取用户输入的信息，处理完成后，再发送ajax请求

~~~
<button @click="send()">注册</button>
~~~

方式二：给form标签添加一个提交事件，在函数中获取用户输入的信息，处理完成后，再发送ajax请求

~~~html
    <form @submit.prevent="send()">
~~~

##### 将收集好的数据发送给服务器

将所有的数据进行拼接，发送到服务器，可以使用JSON.stringify（） 将对象转换为字符串，但是data中可能含有除了表单以外的数据，所有建议在data中新建一个对象，专门存储表单中的数据

~~~javascript
methods:{
            send(){
                // alert("ajax......")
                //将收集好的数据发送给服务器
                JSON.stringify(this.data)
            }
        }
~~~

#### 4.14过滤器（filters 配置项）

过滤器filter 使用于简单的逻辑处理，例如：对一些数据进行格式化显示。他的功能完全可以使用methods、computed来实现。过滤器可以进行全局配置，也可以进行局部配置。（在Vue3中已经将过滤器废除了）

​	1.全局配置：在构建任何Vue实例之前使用  Vue.fliter(' 过滤器名称 ', callback )进行配置。

​	2.局部配置：在构建Vue实例的配置项中使用 filters 进行局部配置

~~~html
    <h1>商品价格（局部过滤器){{price|filterA}}</h1>
 	<h1>商品价格（全局过滤器){{price|filterC}}</h1>

<script>
    全局过滤器：
    Vue.filter("filterC",function (value) {
        return value
    })
    局部过滤器：
    const vm = new Vue({
        el:"#app",
        data:{
            price:50.6
        },
        filters:{
            filterA(val){
                if (val===''||val===undefined||val===null){
                    return "-"
                }
                return val
            }
        }
    })
</script>
~~~

**原理**：将price传递给过滤器，过滤器中的filterA函数的val参数负责接受price的值

过滤器可以用在两个地方：**差值语法** 和 **v-bind 指令**中

~~~html
差值语法： 
<h1>商品价格（过滤器){{price|filterA}}</h1>
v-bind指令中：
<input type="text" :value="price|filterA">
~~~

**多个过滤器可以串联**：{{ msg | filterA | filterB | filterC }}

​		数据传递顺序：将msg传递给filterA ，filterA 返回的数据传递给filterB，filterB 将返回的数据再给filterC，最后渲染



过滤器也可以接受额外的参数，但过滤器的第一个参数永远接受的都是一个过滤的返回值。

~~~javascript
//val 永远是上一个过滤器返回的值，或者原本需要过滤的数据
filterB(val,number){
                return number
            }

使用：
    <h1>商品价格（多个参数的过滤器){{price|filterB(2)}}</h1>
~~~

#### 4.15 Vue的其他指令

##### v-text

​	作用：将内容填充到标签体中，并且是以**覆盖**的形式填充，而且填充的内容中即使存在HTML标签也只是会当做一个普通的字符串处理，不会解析。功能等同于原生js中的innerText

~~~~html
<div id="app">
    <h1>{{message}}</h1>
    <h1 v-text="'v-text='+name"></h1>
    <h1 v-text="'v-text='+htmlcode1"></h1>
</div>
<script>
    const vm = new Vue({
        el:"#app",
        data:{
            message:"其他指令",
            name:"张三",
            htmlcode1:'<span>使用v-text指令</span>',
        }
    })
</script>
~~~~

##### v-html

将内容填充到标签体中，并且是以**覆盖**的形式填充，而且将填充的内容当做HTML代码解析。功能等同于js中的innerHTML。

```html
<div id="app">
    <h1>{{message}}</h1>
    <h1 v-html="'v-text='+htmlcode2"></h1>
</div>
<script>
    const vm = new Vue({
        el:"#app",
        data:{
            message:"其他指令",
            htmlcode2:'<span style="color: red">使用v-html指令</span>'
        }
    })
</script>
```

​	z注意：v-html 不要用到用户提交的内容上。可能会导致XXS攻击、XXS攻击通常指的是通过利用网页开发时留下的漏洞，通过巧妙的方法注入恶意指令代码到网页。使用户加载并执行攻击者恶意制造的网页程序。这些恶意网页程序通常是javaScript.

##### v-cloak

v-cloak  配置css样式来解决胡子的闪现问题（就是网络差的时候，还没来得及加载Vue，导致插值语法没有编译，直接显示）。

v-cloak 指令使用在标签当中，当Vue实例接管之后会删除这个指令。

​	需要配合css样式一起使用：

~~~html
    <style>
        [v-cloak]{
            display: none;
        }
    </style>
    <h1 v-cloak>{{'v-cloak'+message}}</h1>
~~~

虽然设置了display: none; 但是当页面加载到Vue实例的时候，Vue框架会自动识别，让该样式失效

##### v-once 

只渲染一次，之后将被视为静态内容。

##### v-pre

使用该指令可以提高编译速度。带有该指令的标签将不会被编译。可以在没有Vue语法规则的标签中使用可以提高效率。不要将它用在带有指令语法以及插值语法的标签中。

~~~html
    <h1 v-pre>{{'v-pre'+message}}</h1>  <!--{{'v-pre'+message}}-->
~~~

#### 4.16 自定义指令（directives配置项）

在Vue可以自定义指令

指令的命名规则：1.不需要是  v-Xxx  的形式

​								2.Vue官方建议指令的名字要全部是小写，如果是多个单词的话，使用 ' - ' 进行连接

##### 函数式自定义指令（局部）

语法格式：

~~~javascript
directives:{
            '自定义指令名':function (x,y) {

            }
        }
~~~

这个回调函数有两个参数：第一个参数是真实的dom元素，第二个参数就是标签与指令之间绑定关系的对象。

回调函数的执行时机：1.在标签和指令第一次绑定的时候，2.模板被重新解析的时候

~~~html
<div id="app">
    <!--自定义指令-->
    <h1 v-text-danger="message"></h1>
</div>
<script>
    const vm = new Vue({
        el: "#app",
        data: {
            message: "自定义指令"
        },
        directives: {
            'text-danger': function (element,binding) {
                console.log(element)
                console.log(binding)
                element.innerText=binding.value //value 就是 message的值
                element.style.color='red'
            }
        }

    })
</script>
~~~

![image-20230717173042634](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230717173042634.png)

为什么根据 element 获取父级元素为null？

~~~javascript
directives: {
            'text-danger': function (element,binding) {
                console.log(element.parentNode) //null
            }
        }
~~~

​		因为这个函数在执行的时候，指令和元素完成了绑定，但是只是在内存当中完成了绑定，元素还没有被插入到页面当中

##### 对象式自定义指令（局部）

在对象式自定义指令中，自定义指令中有三个固定的函数，分别是bind、inserted、update ，这三个函数将来都会被自动调用。

语法格式：

~~~javascript
 directives: {    
            '自定义指令名':{
                //元素与指令初次绑定的时候，自动调用bind
               // 注意：在特定的时间节点调用特定的函数，这种被调用的函数称为钩子函数
                bind( element,binding ){
                    
                },
                 //元素被插入到页面之后，这个函数会被自动调用
                inserted( element,binding ){
                    
                },
               // 当模板重新解析的时候，这个函数被自动调用
                update( element,binding ){
                    
                }
            }
        }
~~~

##### 全局自定义指令

~~~javascript
    // 全局对象形式
    Vue.directive('bind-blue',{
        bind(element,binding){
 
        },
        inserted(element,binding){

        },
        update(element,binding){

        }
    })
    //全局函数形式
    Vue.directive('bind-blue',function (element,binding) {
        
    })
~~~

##### 自定义指令中的this

对于自定义指令来说，函数体中的this是window，而不是Vue实例

#### 4.17 响应式与数据劫持

 	响应式定义：修改data后，页面自动改变/刷新。这就是响应式。就像我们在使用excel的时候，修改一个单元格中的数据，其他单元格							的数据会联动更新

​	 数据劫持：Vue底层使用了Object.defineProperty,配置setter方法，当去修改属性值时，setter方法则被自动调用，setter方法中不仅修改了属性值，而且还做了其他的事情，例如：重新渲染页面。setter方法就像半路劫持一样，所以成为数据劫持。

**<u>Vue会给data中所有的属性，以及属性中的属性，都会添加响应式</u>**

##### 添加响应式属性

​			语法格式：	方式一：***Vue. set( 目标对象 ，‘ 属性名 ’， 属性值  )***

​									方式二：***Vue. $set(目标对象，‘ 属性名 ’，属性值)***

~~~javascript

    const vm = new Vue({
        el:"#app",
        data:{
            message:"响应式与数据劫持",
            a:{}
        }
    })
    //方式一：
    Vue.set(vm.$data.a,'name','张三')
	//方式二
	Vue.set(vm.a,'name','张三')
    console.log(vm)
~~~

**注意**：目标对象只能是data中的JSON对象，否则会报错，并且不能针对data配置项添加属性，例如：Vue.set(vm.data,'a','b')（这样会报错）

##### 通过数组下标能否做到响应式处理？

~~~javascript

    const vm = new Vue({
        el:"#app",
        data:{
            arrays:['jack','lucy','james'],
            arrayObjects:[
                {id:'111',name:'zhangsan'},
                {id:'222',name:'lisi'}
            ]
        }
    })
~~~

​	问题：在控制台中输入 vm.$data.arrays[0]='ja' 修改数组中的值，页面没有重新渲染。但是输入vm.$data.arrayObjects[0].name='jack' 可以做到响应式处理，页面重新渲染

​	说明：如果该数组不是JSON数组，通过数组的下标去修改数组中的元素，默认情况下是没有添加响应式处理的。但是，如果数组中含				有多个JSON对象，那么通过数组的下标修改数组中某个JSON对象的属性，是可以做到响应式处理的

##### 解决：数组的响应式处理

​	方案一：通过数组下标，<u>修改数组中的值</u>，完成响应式

 				语法：***vm.$set( 数组对象 , 下标 , 值 )		或者		vm.set( 数组对象 , 下标 , 值 )***

​	方案二：调用Vue提供的7个API

​					push(' 需要添加的值 ')					向数组末尾添加数组

​					pop()												删除数组末尾元素

​					reverse()											反转数组

​					splice(下标，个数，‘要修改的值')					修改

​					shift()				 								删除数组中的第一个元素

​					unshift(' 需要添加的值 ')				向数组首部添加元素

​					sort()												排序

#### 	4.18 Vue的生命周期

Vue 的生命周期：

> ​			1.虚拟DOM在内存中就绪时：去调用一个a函数
>
> ​			2.虚拟DOM转换为真实DOM渲染到页面时：去调用一个b函数
>
> ​			3.Vue的data发生改变时，去调用c函数
>
> ​			4.。。。。
>
> ​			5.Vue 实例被销毁时：去调用一个x函数
>
> ​	在生命线上的函数叫做钩子函数，这些函数是不需要程序员手动调用的，由Vue自动调用，程序员只需要按照自己的需求写上，到了哪	个时间点自动就会执行
>

##### 掌握Vue生命周期的作用

> 研究Vue生命周期主要研究的核心是：在哪个时刻调用了哪个钩子函数
>



##### Vue生命周期的4个阶段8个钩子

------

Vue的生命周期可以被划分为4个阶段：**初始阶段、挂载阶段、更新阶段、销毁阶段**。

每个阶段会调用两个钩子函数。两个钩子函数的特点：beforeXxx( )、xxxed( )

8个生命周期钩子函数分别是：

​	1.初始阶段

​			beforeCreate()		  创建前

​			created()					创建后

​	2.挂载阶段

​			beforeMount() 		挂载前

​			mounted()				挂载后

​	3.更新阶段

​			beforeUpdate()		 更新前

​			updated()					更新后

​	4.销毁阶段

​			beforeDestroy()			销毁前

​			destroyed()					销毁后

~~~javascript
//直接写在Vue实例中即可
    const vm = new Vue({
        el: "#app",
        data: {
            message: "响应式与数据劫持",
            number:0
        },
        methods:{
            add(){
                if (this.number == 10) {
                    this.$destroy()
                }
                this.number++;

            }
        },
        // 1.初始阶段
        beforeCreate() {// 创建前
            console.log("beforeCreate")

        },
        created() { // 创建后
            console.log("created")
        },

        // 2.挂载阶段
        beforeMount() { // 挂载前
            console.log("beforeMount")
        },
        mounted() {// 挂载后
            console.log("mounted")
        },

        // 3.更新阶段
        beforeUpdate() {// 更新前
            console.log("beforeUpdate")
        },
        updated() {// 更新后
            console.log("updated")
        },

        // 4.销毁阶段
        beforeDestroy() {// 销毁前
            console.log("beforeDestroy")
        },
        destroyed() {// 销毁后
            console.log("destroyed")
        }

    })


~~~

##### Vue生命周期图析

![屏幕截图 2023-07-18 201531](C:\Users\fengshun\Desktop\自学\Vue\视屏\屏幕截图 2023-07-18 201531.png)

![屏幕截图 2023-07-18 201629](C:\Users\fengshun\Desktop\自学\Vue\视屏\屏幕截图 2023-07-18 201629.png)

##### 初始阶段

> ​		创建前：指的是数据代理和数据检测的创建前，此时还无法访问data当中的数据。包括methods也是无法访问的。
>
> ​		创建后：表示数据代理和数据检测创建完毕，可以访问data以及methods中的数据了

作用：

> ​	**beforeCreate**：可以在此时加一些 loading 效果。
>
> ​	**created**：结束 loading 效果。也可以在此时发送一些网络请求，获取数据。也可以在这里添加定时器。

##### 挂载阶段

> ​		挂载前：生成虚拟DOM，在此时如果修改标签的值，页面会暂时发生改变，待挂载完毕之后，生成的真实的DOM会覆盖				掉以前的虚拟DOM，页面会恢复到原样
>
> ​		挂载后：页面渲染成功，此时再修改DOM 页面也会随之发生修改

作用：

> **mounted**：可以操作页面的 DOM 元素了。

##### 更新阶段

​		注意：当data发生改变时，才会进入更新阶段吗，同理，必须要手动调用destroy方法，才会进入销毁阶段。

作用：

> ​	**beforeUpdate**：适合在更新之前访问现有的 DOM，比如手动移除已添加的事件监听器。
>
> ​	**updated**：页面更新后，如果想对数据做统一处理，可以在这里完成。

##### 销毁阶段

​		**销毁阶段**是指卸载监视器、子组件、自定义事件监听器，Vue实例依旧存在。<u>如果是Vue高版本还会移除事件监听器</u>

​		注意：在销毁前，不能使用Vue中的属性和方法，如果在beforeDestroy函数中，更改data中的数据，只是会让内存中的数					据变化，但是页面不会被重新渲染

***手动销毁Vue：Vue实例.$destroy()***

作用：

​	**beforeDestroy**：适合做销毁前的准备工作，和人临终前写遗嘱类似。例如：可以在这里清除定时器

### 5 Vue 组件化

####    5.1 什么是组件

------

(1) 传统方式开发的应用

​				一个网页通常包括三部分：结构（HTML）、样式（CSS）、交互（JavaScript）

<img src="C:\Users\fengshun\Desktop\自学\Vue\视屏\屏幕截图 2023-07-18 224841.png" alt="屏幕截图 2023-07-18 224841" style="zoom:50%;" />

传统应用存在的问题：

> ​			1 .关系纵横交织，复杂，牵一发动全身，不利于维护。
>
> ​			2.代码虽然复用，但复用率不高



(2) 组件化方式开发的应用

使用组件化方式开发解决了以上的两个问题：

> ​			1.每一个组件都有独立的 js，独立的 css，这些独立的 js 和 css 只供当前组件使用，不存在纵横交错。
>
> 更加便于维护
>
> ​			2.代码复用性增强。组件不仅让 js css 复用了，HTML 代码片段也复用了（因为要使用组件直接引入
>
> 组件即可）。

**组件定义：**

> 1. 组件：实现应用中局部功能的代码和资源的集合。凡是采用组件方式开发的应用都可以称为组件化应用。
>
> 2. 模块：一个大的 js 文件按照模块化拆分规则进行拆分，生成多个 js 文件，每一个 js 文件叫做模块。凡
>
>    ​			是采用模块方式开发的应用都可以称为模块化应用。
>
> 3. 任何一个组件中都可以包含这些资源：HTML CSS JS 图片 声音 视频等。从这个角度也可以说明组件
>
>    是可以包括模块的。

**注意**：组件的划分粒度很重要，粒度太粗会影响复用性。为了让复用性更强，Vue 的组件也支持父子组件嵌套使用。



#### 5.2组件的创建、注册与使用

------

##### **第一步：创建组件**

​		语法：***Vue.extend({该被配置项和new Vue 的配置项几乎相同，但略有差别})***

区别：

> 1. 创建组件的时候，配置项中不能出现 el 配置项，但是<u>需要使用template配置项来配置模板语句</u>
> 2. 配置项中的data不能使用直接对象的形式，必须使用函数形式

~~~javascript
 //创建组件
    const myConponent = Vue.extend({
        template: "    <ol>\n" +
            "        <li v-for=\"user of users\" :key=\"user.id\">{{user.id}},{{user.name}}</li>\n" +
            "    </ol>",
        data() {
            return {
                users: [
                    {id: '001', name: 'jack'},
                    {id: '002', name: 'lucy'},
                    {id: '003', name: 'james'}
                ]
            }
        }
    })
    //省略版：直接定义一个对象
    const myConponent ={
        template: "    <ol>\n" +
            "        <li v-for=\"user of users\" :key=\"user.id\">{{user.id}},{{user.name}}</li>\n" +
            "    </ol>",
        data() {
            return {
                users: [
                    {id: '001', name: 'jack'},
                    {id: '002', name: 'lucy'},
                    {id: '003', name: 'james'}
                ]
            }
        }
    }
~~~

------

##### **第二步：注册组件**（components配置项）

局部注册：

> ​			在配置项中使用 **components** 配置项。
>
> 语法格式：
>
> ​						components：{
>
> ​											组件的名字：组件对象，
>
> ​						}

~~~javascript
    //Vue实例
    const vm = new Vue({
        el: "#app",
        data: {
            msg: '组件化开发',
        },
        //注册组件(局部注册）
        components : {
            userlist : myConponent
        }
    })
~~~

全局注册：

> 语法： **Vue.component(' 组件的名字 ',组件对象)**

~~~javascript
    Vue.component('userlongin',userLogin)
~~~

**注意**：局部注册和全局注册的组件都只能在Vue管理的实例中使用，不能在外单独使用

------

##### **第三步：使用组件**

直接将定义好的组件名写成html形式即可

注意：

> 1.组件的名字必须全部为小写字母，如果出现大写字母就会报错。
>
> 2.在Vue实例中可以使用自闭合标签，但是必须在脚手架环境中使用

~~~html
<div id="app">
    <userlist></userlist>
</div>
~~~

总体案例：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>第一个组件</title>
    <script src="../../js/vue.js"></script>
</head>
<body>
<div id="app">
    <userlogin></userlogin>
</div>
<hr>
<div id="app2">
    <login></login>
</div>

<script>
    //创建组件
    const userLogin = Vue.extend({
        template: '    <div>\n' +
            '    <h3>用户登录</h3>\n' +
            '    <form @submit.prevent="login">\n' +
            '        账号：<input type="text" v-model="username"><br>\n' +
            '        密码：<input type="password" v-model="password"><br>\n' +
            '        <button>登录</button>\n' +
            '    </form>\n' +
            '    </div>',
        data() {
            return {
                username: '',
                password: ''
            }
        },
        methods: {
            login() {
                console.log(this.username + '=' + this.password)
            }
        }
    })
    //全局注册
    Vue.component('login',userLogin);
    //Vue实例
    const vm = new Vue({
        el: "#app",
        data: {
            msg: '组件化开发',
        },
        //注册组件(局部注册）
        components: {
        //局部注册
            userlogin: userLogin
        }
    })

</script>
~~~



##### 组件的命名规则

方式一：全部小写

方式二：首字母大写，后面都是小写

方式三：串式命名法：例如user-login

方式四：驼峰式命名法：例如UserLogin  但是这种方式只允许在脚手架环境中使用

#### 5.3组件的嵌套

定义组件A和组件B，将组件A注册到Vue实例中，再将组件B注册到组件A中，即可实现组件挂载。需要注意的是，在哪里注册的组件，就只能在其父组件的template中使用。

~~~html
<div id="app">
    <x></x>

</div>
<script>
    //组件B
    const myConponent = Vue.extend({
        template: "    <ol>\n" +
            "        <li v-for=\"user of users\" :key=\"user.id\">{{user.id}},{{user.name}}</li>\n" +
            "    </ol>",
        data() {
            return {
                users: [
                    {id: '001', name: 'jack'},
                    {id: '002', name: 'lucy'},
                    {id: '003', name: 'james'}
                ]
            }
        }
    })

    //创建组件A
    const  x=Vue.extend({
        template:'<div>' +
            '<h1>APP组件</h1>' +
            '<userlist></userlist>' +
            '</div>',
        //将B组件注册到A组件中
        components:{
            userlist:myConponent
        }
    })

//Vue实例
    const cm = new Vue({
        el:"#app",
        data:{
            msg:'组件嵌套'
        },
        //将A组件注册到Vue实例中
        components:{x}
    })
</script>
~~~

#### 5.4  VueComponent

##### 5.5.1 this

> 1.在new Vue({ }) 配置项中的this就是 Vue实例（vm)
>
> 2.Vue.extends({ })配置项中的this就是Vuecomponent 实例(vc)
>
> 3.vm 与 vc 具有大量相同的属性和方法，比如生命周期钩子、methods、watch等。
>
> 4.但是vm与vc并不是完全相同的，vc上面有的，vm上面一定又，但是vm上面有的，vc不一定有，比如vm中有el，而vc中没有

##### 5.5.2 Vue.extends 

> 1.<u>每一次的 extend 调用返回的都是一个全新的 VueComponent 构造函数</u>
>
> 2.返回的构造函数在html标签中引用的时候被实例化，如 Vue在解析<login></login>时会创建一个 VueComponent  实例，也就是new VueComponent ( )
>
> ~~~javascript
>     const  x=Vue.extend({
>     })
>     通过定义一个变量来接受这个 VueComponent 构造函数，等到html中引用时，Vue在编译html时会自动创建VueComponent 实例

##### 5.5.3 回顾原型对象

**prototype** 称为：显示的原型属性，用法：函数.prototype，例如：Vue.prototype 

__ **proto** __ 称为：隐式的原型属性，用户：实例. __ proto __ ，例如：vm. __ proto __ 

> 无论是通过 prototype 还是__proto__，获取的对象都是同一个，它是一个共享的对象，称为：XX 的原型对象。
>
> 如果通过 Vue.prototype 获取的对象就叫做：Vue 的原型对象。
>
> 如果通过 User.prototype 获取的对象就叫做：User 

<img src="C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230720103558812.png" alt="image-20230720103558812" style="zoom:80%;" />

##### 5.5.4解剖

VueComponent.prototype.__proto__ = Vue.prototype

![image-20230720143420969](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230720143420969.png)

#### 5.5 单文件组件

1.什么是单文件组件？

> (1) 一个文件对应一个组件（之前我们所学的是非单文件组件，一个 html 文件中定义了多个组件）
>
> (2) 单文件组件的名字通常是：x.vue，这是 Vue 框架规定的，只有 Vue 框架能够认识，浏览器无法直接打
>
> 开运行。需要 Vue 框架进行编译，将 x.vue 最终编译为浏览器能识别的 html js css。
>
> (3) 单文件组件的文件名命名规范和组件名的命名规范相同：
>
> ​		1.全部小写：userlist
>
> ​		2.首字母大写，后面全部小写：Userlist
>
> ​		3.kebab-case 命名法：user-list
>
> ​		4.CamelCase 命名法：UserList（我们使用这种方式，和 Vue 开发者工具呼应。）

2. x.vue 文件的内容包括三块：

   - 结构：<template>HTML 代码</template>
   - 交互：<script>JS 代码</script>
   - 样式：<style>CSS 代码</style>

3.  export 和 import，ES6 的模块化语法。

   使用 export 导出（暴露）组件，在需要使用组件的 x.vue 文件中使用 import 导入组件

   > 1. 默认导入和导出
   >    - export default {}
   >    - import 任意名称 from ‘模块标识符’
   > 2. 按需导入和导出
   >    - export {a, b}
   >    - import {a, b} from ‘模块标识符’
   > 3. 分别导出
   >    - export var name = ‘zhangsan’
   >    - export function sayHi(){}

~~~html
<!--结构-->
<template>
    <div>
        <h3>用户登录</h3>
        <form @submit.prevent="login">
            账号：<input type="text" v-model="username"><br>
            密码：<input type="password" v-model="password"><br>
            <button>登录</button>
        </form>
        <hr>
        <userlist></userlist>
    </div>
</template>
<!--样式-->
<style></style>
<!--交互-->
<script>
    // 导入组件
    import userlist from "./userlist.Vue";
    // 导出
    export default {
        data() {
            return {
                username: '',
                password: ''
            }
        },
        methods: {
            login() {
                console.log(this.username + '=' + this.password)
            }
        },
        // 注册组件
        conmponents:{userlist}
    }
</script>
~~~

#### 5.6脚手架

##### 创建项目

1.创建项目

​		项目中自带脚手架环境，自带一个HelloWord案例

> 在dos命令窗口，cd到需要创建脚手架的文件中，输入<u>**vue create 项目名**</u>
>
> 选择 Vue2，babel：负责 ES6 语法转换成 ES5，eslint：负责语法检查的。
>
> 回车之后，就开始创建项目，创建脚手架环境（内置了 webpack loader），自动生成 HelloWorld 案例。

2.编译Vue程序

> dos 命令窗口中切换到项目根：**<u>cd 项目名</u>**
>
> 执行：**<u>npm run serve</u>**，这一步会编译 HelloWorld 案例
>
> (ctrl + c 停止服务)

##### 认识脚手架结构

![image-20230720165537939](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230720165537939.png)

package.json：包的说明书（包的名字，包的版本，依赖哪些库）。该文件里有 webpack 的短命令：

​		serve（启动内置服务器）

​		build 命令是最后一次的编译，生成 html css js，给后端人员

​		lint 做语法检查的。

##### 分析HelloWord程序

1.index.html

~~~html
<!DOCTYPE html>
<html lang="">
  <head>
    <meta charset="utf-8">
    <!-- 让IE浏览器启用最高渲染标准，IE8是不支持Vue的 -->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!-- 开启移动端虚拟窗口（有的人称为理想窗口） -->
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <!-- 设置页签图表 -->
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <!-- 设置标题 -->
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
    <!-- 当浏览器不支持js语言的时候，显示如下信息 -->
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>
    <!-- 容器 -->
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>


<!-- 
在idnex.html中：
  没有看到引入vue.js文件
  也没有看到引入main.js文件。Vue脚手架会自动找到main.js文件，不需要手动引入
  所以main.js文件的名字不要随意修改，mian.js的文件位置也不要随意变动 -->
~~~

2.mian.js

~~~javascript
// 等同于引入vue.js文件
import Vue from 'vue'
// 导入app组件
import App from './App.vue'

// 关闭生产提示信息
Vue.config.productionTip = false

// 创建Vue实例
new Vue({
  render: h => h(App),
}).$mount('#app')

~~~

在Vue项目中，主要有四个文件：index.html 、app.vue 、HelloWord.vue 、mian.js

HelloWord.vue 是子组件，嵌套在app的组件中，通过main.js 创建 vue 实例并注册组件，在index.htrml中 新建id为app的标签，让vue实例去管理该容器



##### 使用项目

> 在dos命令窗口，按下ctrl+c 两次可停止项目的运行，在修改完项目代码后，都需要输入 npm run serve 重新编译程序
>
> 注意：如果出现 如下错误‘Component name "userlist" should always be multi-word’ 可在vue.config.js中添加 ‘lintOnSave: false’ 
>
> 取消自动语法检查



##### 解释mian.js中的render函数

目前使用的vue.js是一个缺失了模板编译器的vue.js文件，解决方案：

> 第一种：使用完整版的vue.js
>
> ​	在node_modules/dist/vue.runtime.esm.js 中 修改：import Vue from 'vue/dist/vue.js'
>
> 第二种：使用render函数
>
> ​	在render函数被调用的时候，会自动传入一个参数：createElement，createElement是一个函数，它可以创建元素

为什么采用缺失模板编译器的vue.js

> 目的：减少体积、vue.js包括两块：vue的核心 + 模板编译器（模板编译器可能占用vue.js文件体积的三分之一）
>
> 将来程序员使用webpack 进行打包处理之后，模板编译器就没有存在的必要了。

#### * 5.7 props 配置

使用 props 配置可以接收其他组件传过来的数据，让<u>组件的数据变为动态数据</u>，三种接收方式：

> 1.简单接收：
>
> ​			props:[ ' 属性名1 ',' 属性名2 ',' 属性名3 ']
>
> ~~~javascript
> props:['msg','name','password']
> ~~~
>
> 2.接收时添加类型限制：
>
> ​			props:{
>
> ​					属性名1：数据类型，
>
> ​					属性名2：数据类型
>
> ​			}
>
> ~~~javascript
>   props: {
>     msg: String,
>     users: Array
>   }
> ~~~
>
> 3.接收时添加数据限制，并且还可以添加必要性限制，添加默认值
>
> ​			props:{
>
> ​					属性名1:{
>
> ​							type : String,			------数据类型
>
> ​							requried : true,		 ------必要性
>
> ​							default : '默认值'		------默认值	
>
> ​						}
>
> ​			}
>
> ~~~javascript
>   props: {
>     msg: {
>       type: String,
>       requried: true,
>       default: '默认值'
>     }
>   }

父组件向子组件传递数据：

~~~html
<!--传递普通的字符串-->
<user name='张三' password='0124'></user>
<!--通过 v-bind 传递常量、表达式等-->
<userlist :users="[{ id: '00', name: 'jack' },{ id: '002', name: 'lucy' },{ id: '003', name: 'james' }]">	</userlist>
~~~

**注意**：不要修改props中属性的值，因为父组件每次刷新的时候，会重新渲染，覆盖修改后的值。如果需要修改，可以将需要操作的属性			传递给一个新的变量，再对该变量进行操作

#### 5.8 从父组件中获取子组件（ref）

第一步，打标记：

> 通过 ref 属性标记，如果在普通的标签中设置该属性，则获取的是一个DOM元素，如果设置在子组件上，则获取的是Vue实例
>
> ​	语法： **ref=' 标记名 '**
>
> ~~~html
> <!--获取DOM元素-->
> <h3 ref="h">用户登录</h3>
> <!--获取Vue实例-->
> <userlist :users="[{ id: '00', name: 'jack' },{ id: '003', name: 'james' }]" ref="u"></userlist>

第二步，访问：

> 通过 this.$refs.标记名 获取DOM元素或者子组件对象
>
> ​	语法： **this.$refs.标记名**
>
> ~~~javascript
>     usersIofo() {
>       console.log(this.$refs.h)			//获取DOM对象
>       console.log(this.$refs.u)			//获取子组件对象 vc
>       console.log(this.$refs.u.users)	 //访问子组件对象 vc 中的属性的值
>     }

#### 5.9 mixins 配置（混入）

当多个组件中有相同的 methods 、data等配置项 时，为了提高代码的复用，可以使用 mixins 配置。

第一步：在src下面 创建 mixin.js 文件

~~~javascript
export const mixin1 = {
    data(){
        return{
            //数据
        }
    },
    methods: {
        函数名() {
            //函数体
        }
    }
}
~~~

第二步：在需要使用函数的组件中引入并使用

引入：

~~~javascript
import { mixin1 } from '../mixin.js';
~~~

使用：

​	数组表示，可以使用多个mixin

~~~javascript
  mixins: [mixin1]
~~~

​	直接在标签中使用定义好的方法名即可

~~~html
<button @click="get">mixins的使用</button>
~~~

------

当子组件中自定义的方法名和mixin中定义的方法名相同时，会发生覆盖吗？

> 不会，当两者方法名冲突时，只会使用子组件中定义的方法，不会再次使用mixin中定义的方法

当子组件中定义了生命周期中的八个钩子函数，mixin中也定义了相同的钩子函数，会发送覆盖吗？

> 不会，会先执行mixin中的钩子函数，然后再执行子组件中定义的钩子函数

全局导入

> 作用：让所有的组件都能共享mixin中定义的函数，无需分别导入与使用
>
> ​	操作：在**mian.js**中导入mixin，并且引用即可
>
> ~~~javascript
> //导入
> import { mixin1 } from './mixin.js';
> import { mixin2 } from './mixin.js';
> //引用
> mixins:[mixin1,mixin2]
> //如果mixin中定义了生命周期中的8个钩子函数，那么在全局导入之后，所有的组件都会启用该钩子函数



#### 5.10 plugins 配置（插件）

​	作用：给Vue做功能增强的

​	插件是一个对象，对象中必须有install方法，这个方法会被自动调用。在插上插件之后，刷新页面，系统会自动调用install方法

定义插件：

~~~javascript
//每一个插件都是一个对象
exprot const plugin1 = {
    /*    每一个插件对象中必须有一个install方法
       这个install方法会被自动调用
       install方法中的参数，包括两个部分：
       第一部分：Vue构造函数
       第二部分：可以接收用户在使用这个插件是，传过来的数据，参数个数无限
       */
    install(Vue, a, b, c) {
        console.log("使用插件：" + a + ',' + b + c)
    }
}
~~~

使用插件：

在main.js中导入插件

~~~javascript
//导入插件
import plugin1 from './plugins.js'
//插件的使用，通常放在创建Vue实例之前
//use就是插上插件，删除该行就是拔下插件
Vue.use(plugin1)

//传递参数
Vue.use(plugin1, '参数1', '参数2', '参数3')
~~~

#### 5.11 局部样式 scoped

默认情况下，在 vue 组件中定义的样式最终会汇总到一块，如果样式名一致，会导致冲突，冲突发生后，以后来

加载的组件样式为准。

为了让多个组件之间的样式互不干扰，可以使用 scoped 修饰

使用：

~~~html
<style scoped>
//定义样式
</style>
~~~

注意：一般在APP根组件中的样式不会添加 scoped 因为根组件还是希望采用全局的方式处理。

#### 5.12 组件自定义事件

​	应用：可以做到子组件向父组件传递数据

##### 第一种方式：直接在组件标签上绑定事件

------

1.内置事件的实现步骤

> 第一步：提供事件源
>
> 第二步：给事件源绑定事件（v-on：事件名 或者 @事件名）
>
> 第三步：编写回调函数，将回调函数和事件进行绑定
>
> 第四步：等待事件的触发，只要事件触发，则执行回调函数

2.关于组件的自定义事件的实现步骤

> 第一步：提供事件源，(这个事件源是一个组件)
>
> 第二步：给事件源绑定事件（v-on：事件名 或者 @事件名）
>
> 第三步：编写回调函数，将回调函数和事件进行绑定
>
> 第四步：等待事件的触发，只要事件触发，则执行回调函数
>
> ​		对于组件自定义事件来说，要想事件发生，需要去执行一段代码。这段代码负者去触发这个事件，让这个事件发生



1.提供事件源、绑定事件

~~~html
    <BugFooter @event1="eventFunction"></BugFooter>
	<!--保证事件只触发一次-->
	<BugFooter @event1.once="eventFunction"></BugFooter>
~~~

2.编写回调函数

~~~javascript
eventFunction() {
    //编写触发event1事件的后处理的函数
    }
~~~

3.在子组件中定义触发event1事件的函数

~~~javascript
triggerEvent1() {
    //编写触发event1事件的后处理的函数
    //this是当前的组件实例
    this.$emit('event1')
    }
~~~

4.在子组件中定义按钮，用于触发该事件

~~~html
<button @click='triggerEvent1'> 触发event1事件</button>
~~~

​		原理：在子组件中点击按钮，触发事件，执行 triggerEvent1 函数，在函数体中 执行了 this.$emit('event1') ，使得父组件中的自定义的事件触发了，自定义事件触发即执行了 eventFunction 函数

<u>触发事件的同时可以给事件绑定的回调函数传数据</u>（数据或对象）

~~~javascript
triggerEvent1() {
    //编写触发event1事件的后处理的函数
    //this是当前的组件实例，传递需要的数据给父组件中的回调函数
    this.$emit('event1',this.name,this.age,this.gender)
    }
~~~

App.vue 源代码：

![image-20230721211715728](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230721211715728.png)

user.vue 源代码：

<img src="C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230721211917882.png" alt="image-20230721211917882" style="zoom:80%;" />

总结：

> 到目前为止，父子组件之间如何通信
>
> 父----子：
>
> ​			props
>
> 子----父：
>
> ​			第一种方式：在父中定义一个方法，将方法传递给子，然后在子中调用父传过来的方法。这样给父传递数据。（这种方式								  以后很少使用） props
>
> ​			第二种方式：使用组件的自定义事件的方式，也可以完成子向父传递数据。
>
> ​						APP是父组件，user是子组件
>
> ​				子组件向父组件传递数据（User给APp传递数据）：
>
> ​						在父组件中绑定事件，在子组件当中触发事件，子组件可以向父组件传递数据
>

##### 第二种方式：通过代码给组件绑定事件

在父组件中绑定事件

> 1.准备一个组件
>
> ~~~html
>     <user ref="user"></user>
> ~~~
>
> 2.给子组件绑定event1事件，并且给event1事件绑定一个回调函数：eventFunction
>
>  		**语法：this.$ref.标记名.$on(' 事件名 '，回调函数 )**
>
> ~~~javascript
> 
> import user from './components/user.vue'
> export default {
>   name: 'App',
>   components: {
>     user
>   }, 
>     //挂载后
>     mounted(){
>         //给ref=‘user’的组件绑定event1事件，并且给event2事件绑定一个回调函数：eventFunction
>     this.$refs.user.$on('event1', this.eventFunction)
>   },
>   methods: {
>     eventFunction(name, age, gender) {
>       console.log("666666");
>       console.log(name, age, gender)
>     }
>   }
> }
> ~~~
>
> 3.在子组件中定义按钮和函数，触发event1 事件
>
> ​		定义点击按钮，用于触发 triggerEvent 函数：
>
> ~~~html
>         <button @click="triggerEvent">触发event1事件</button>
> ~~~
>
> ​		triggerEvent 函数执行后，触发 event1 事件：
>
> ~~~javascript
>     methods: {
>         triggerEvent() {
>             this.$emit('event1', this.name, this.age, this.gender)
>         }
>     }
> ~~~
>
> 拓展：保证事件只触发一次。
>
> ~~~javascript
>  mounted() {
> 
>     this.$refs.user.$once('event2', this.eventFunction)
>   },

##### 解绑自定义事件

​	哪个组件绑定的就找哪个组件解绑

​			在app父组件中，给子组件user绑定了一个事件，如果想接触该事件，需要在子组件中进行解除绑定。

​	语法：

​			this.$off( ' 事件名 '，...events )

~~~javascript
    methods: {
        unbinding() {
             this.$off('event1')			      //解绑一个事件
			this.$off( ['event1','event2'] )	 //解绑多个事件
			this.$off()						   //解绑全部事件 
        }
    }
~~~

##### 关于回调函数中的this

1.当回调函数是一个普通函数时，其中的this指向的是子组件，而不是父组件实例

~~~javascript
 mounted() {
    this.$refs.user.$on('event1', function () {
      console.log(this) //指向的是user 这个子组件
    })
  },
      //定义的function函数，是交给user来管理的，所以函数体中的this是指向的user
~~~

2.当回调函数是在父组件中 methods中 的函数，则其中的this指向的是父组件实例

~~~javascript
  methods: {
    eventFunction(name, age, gender) {
      console.log("666666");
      console.log(name, age, gender)
      console.log(this)//指向的是当前组件，即父组件
    }
  }
//因为eventFunction 函数是定义在APP（父组件）的methods配置项中的，methods由APP组件管理，所以this指向的是父组件
~~~

3.当回调函数是箭头函数时，其中的this指向的是父组件APP 实例

~~~javascript
 mounted() {
     this.$refs.user.$on('event1', () => {
      console.log(this)//指向的是当前组件，即父组件
    }) 
  },
~~~

#### 5.13 全局事件总线

作用：解决所有组件之间通信传递数据的问题

组件自定义事件的原理：

> 在APP组件中：this.$refs.user.$on('event2', this.eventFunction) 其中的 this.$refs.user 就是子组件user，而在子组件user中this.$emit('event1', this.name, this.age, this.gender) 其中的this 也是子组件user，意思就是通过user子组件给事件event1绑定回调函数，也是通过user子组件，触发event1事件的。
>
> 子组件user的实例vc 有两个API 分别是 $on(绑定事件) 和 $emit(触发事件)

全局事件总线原理

> ​		给项目中所有的组件找一个共享的 vc 对象。把这个共享的对象 vc 叫做全局事件总线。所有的事件都可以绑定到这个共享对象上。所有组件都通过这个全局事件总线对象来传递数据。这种方式可以完美的完成兄弟组件之间传递数据。
>
> ​	这样的共享对象必须具备两个特征：
>
> ​			1.能够让所有的 vc 共享。
>
> ​			2.共享对象上有$on、$off、$emit 等方法。

方案一：

> ​			在Vue的原型对象上添加一个属性 $bus ，该属性就是一个 VueComponents 对象  vc。使得让这个 vc 共享给所有的组件。在数据传输时，可以通过这个共享的 vc 绑定事件( $on ) 和 触发事件( $emit )
>
> 1.在mian.js中，定义一个共享的vc对象
>
> ~~~javascript
> import Vue from 'vue'
> import App from './App.vue'
> 
> Vue.config.productionTip = false
> 
> 
> //获取VueComponent 构造函数
> const VueComponentConstructor = Vue.extend({});
> 
> //创建一个共享的vc对象
> const vc = new VueComponentConstructor()
> 
> //给Vue的原型对象上扩展一个x属性，x属性指向了这个共享的vc对象
> //给Vue的原型对象扩展的x属性，其他vc都是能访问得到的，这个属性通常为$bus
> Vue.prototype.$bus = vc
> 
> 
> 
> new Vue({
>   render: h => h(App),
> }).$mount('#app')
> ~~~
>
> 2.在需要数据的组件中,绑定事件（在mounted中绑定事件发生的回调函数）
>
> ~~~javascript
> mounted() {
>     //绑定事件，当事件发生后执行 butFunctionBack 回调函数
>     this.$bus.$on('eventx', this.butFunctionBack)
>   },
>  methods: {
>      //定义事件发生的回调函数
>     butFunctionBack(name) {
>       console.log(name)
>     }
>   }
> ~~~
>
> 3.在子组件中定义触发事件的函数
>
> 事件源：
>
> ~~~html
> <button @click="butFunction">触发事件，发送数据给APP</button>
> ~~~
>
> 回调函数：
>
> ~~~javascript
> methods: {
>         butFunction() {
>             this.$bus.$emit('eventx', this.name) //触发eventx 事件，并且传递数据
>         }
>     }

方案二：

> 通过Vue原型对象上添加一个属性，该属性指向VUe实例，通过Vue实例实现共享
>
> 1.在mian.js中，定义一个共享的Vue对象
>
> ~~~javascript
> import Vue from 'vue'
> import App from './App.vue'
> 
> Vue.config.productionTip = false
> 
> new Vue({
>   render: h => h(App),
>     //初始阶段，创建前，对Vue的原型对象添加属性指向Vue实例，这个Vue实例就是自己
>   beforeCreate() {
>     Vue.prototype.$bus = this
>   }
> }).$mount('#app')
> ~~~

#### 5.14 组件之间的数据传输

1.父组件向子组件传递数据：使用props

2.子组件向父组件传递数据：自定义事件

3.多级组件或兄弟组件之间传递数据：全局事件总线



#### 5.15 消息的订阅与发布机制

使用这种机制也可以完成任意组件之间数据的传递，能够完成和全局事件总线一样的功能

![image-20230722180247277](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230722180247277.png)

pubsub.js的安装与使用

> 1.安装 pubsub-js：npm i pubsub-js
>
> 2.程序中引入 pubsub：import pubsub from ‘pubsub-js’
>
> 3.引入了一个 pubsub 对象，通过调用该对象的 subscribe 进行消息订阅，调用 publish 进行消息发布。

------

1.订阅： subscribe

> ​		在挂载完之后实现消息的订阅（mounted函数中）：
>
>  首先，在需要数据的组件中导入pubsub.js ：
>
> ~~~javascript
> import pubsub from 'pubsub-js'
> ~~~
>
> 在该组件中订阅消息：
>
> ~~~javascript
>  mounted() {
>         //A组件挂载完毕之后，订阅消息
>        this.pid = pubsub.subscribe('消息名', function (messageName, message) {
>             //messageName 就是消息的名字
>             //message 就是具体的消息（数据）
>         })
>     }
> }
> 
> 
> //尽量使用箭头函数，实现函数体中的this是Vue实例
> mounted() {
>         //A组件挂载完毕之后，订阅消息
>        this.pid = pubsub.subscribe('消息名', (messageName, message) => {
>             //messageName 就是消息的名字
>             //message 就是具体的消息（数据）
>         })
>     }
> }

2.发布：publish

>    在发布者中导入pubsub.js
>
> ~~~javascript
> import pubsub from 'pubsub-js'
> ~~~
>
> 添加按钮，绑定鼠标单击事件，单击按钮即可发布消息
>
> ~~~html
> <button @click='publishFunction'>发布消息</button>
> ~~~
>
> 定义回调函数
>
> ~~~javascript
>     methods: {
>        publishFunction(){
>            //发布消息
>            pubsub.publish('消息的名字','消息的内容')
>        }
>     },
> ~~~

3.取消订阅

> ~~~javascript
> beforDestory(){
> //当组件实例销毁之前，要取消之前订阅的所有消息
>     pubsub.unsubscribe(this.pid)
> }
> ~~~

点击B组件中的按钮，就会执行 publishFunction 函数，即发布消息，消息发布以后，会自动调用A组件中的函数，

**取消订阅**

​		语法：pubsub.unsubscribe(' 订阅事件的id ')

<u>在订阅消息之后，会返回一个订阅事件的id，通过这个id来取消订阅，可以将这个id存放在Vue实例中。</u>

### 6  Vue 与 Ajax

#### 6.1 回顾发送Ajax异步请求的方式

发送 AJAX 异步请求的常见方式包括：

> 1.原生方式，使用浏览器内置的 JS 对象 XMLHttpRequest
>
> ​		(1) const xhr = new XMLHttpRequest()
>
> ​		(2) xhr.onreadystatechange = function(){}
>
> ​		(3) xhr.open()
>
> ​		(4) xhr.send()
>
> 2.原生方式，使用浏览器内置的 JS 函数 fetch
>
> ​		(1) fetch(‘url’, {method : ‘GET’}).then().then()
>
> 3.第三方库方式，JS 库 jQuery（对 XMLHttpRequest 进行的封装）
>
> ​		(1) $.get()
>
> ​		(2) $.post()
>
> 4.第三方库方式，基于 Promise 的 HTTP 库：axios （对 XMLHttpRequest 进行的封装）
>
> ​		(1) axios.get().then()
>
> axios 是 Vue 官方推荐使用的。

#### 6.2 Aja跨域问题

1.什么是跨域访问？

> （1）在 a 页面中想获取 b 页面中的资源，如果 a 页面和 b 页面所处的**协议、域名、端口**不同（只要有一个不
>
> ​			同），所进行的访问行动都是跨域的。
>
> (2) 哪些跨域行为是允许的？
>
> 1. 直接在浏览器地址栏上输入地址进行访问 
>
> 2. 超链接
>
> 3. <img src="其它网站的图片是允许的">
>
> 4. <link href=”其它网站的 css 文件是允许的”>
>
> 5. <script src=”其它网站的 js 文件是允许的”>
>
> 6. 其他
>
> (3) 哪些跨域行为是不允许的？
>
> 1. AJAX 请求是不允许的
> 2. Cookie、localStorage、IndexedDB 等存储性内容是不允许的
> 3. DOM 节点是不允许的

2.AJAX 请求无法跨域访问的原因：同源策略

> (1) 同源策略是一种约定，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，浏览器很容易受
>
> 到 XSS、CSRF 等攻击。同源是指"协议+域名+端口"三者相同，即便两个不同的域名指向同一个 ip 地址，
>
> 也非同源。
>
> (2) AJAX 请求不允许跨域并不是请求发不出去，请求能发出去，服务端能收到请求并正常返回结果，只是
>
> 结果被浏览器拦截了

3.解决 AJAX 跨域访问的方案包括哪些

> (1) CORS 方案（工作中常用的）
>
> 1. 这种方案主要是后端的一种解决方案，被访问的资源设置响应头，告诉浏览器我这个资源是允许
> 2. 跨域访问的：response.setHeader("Access-Control-Allow-Origin", "http://localhost:8080");
>
> (2) jsonp 方案（面试常问的）
>
> 1. 采用的是<script src="">不受同源策略的限制来实现的，但只能解决 GET 请求。
>
> (3) 代理服务器方案（工作中常用的）
>
> 1. Nginx 反向代理
> 2. Node 中间件代理
> 3. vue-cli（Vue 脚手架自带的 8080 服务器也可以作为代理服务器，需要通过配置 vue.config.js 来启用这个代理）
>
> (4)postMesssage
>
> (5) websocket
>
>  (6)window.name + iframe
>
> (7)location.hash + iframe
>
> (8)document.domain + iframe
>
> (9)其他

4.代理服务器方案的实现原理

​	同源策略是浏览器需要遵循的标准，而如果是服务器向服务器请求就无需遵循同源策略的。

![image-20230723151356436](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230723151356436.png)

#### 6.3 启用 Vue 脚手架内置服务器 8080 的代理功能

安装 axios库： npm i axios

##### 1.简单开启

在vue.config.js 文件中添加配置：

​	这个地址只需要写到端口号即可，不需要写到具体

~~~javascript
  devServer: 
      // 访问 8000 服务器上的资源，访问地址为：http://localhost:8000/veu/bus
    proxy:'http://localhost:8000' //含义：VUe脚手架内置的8080 服务器负责代理访问 8000 服务器
      
  }
~~~

发送ajax请求

导入：

~~~javascript
import axios from 'axios'
~~~

发送Ajax请求：

~~~javascript


//axios.get('http://localhost:8080/veu/bugs').then(
    
    /*可以省略 http://localhost:8080 因为当前就是在8080服务器中，会优先访问自己服务器上的资源，如果没有，再去代理*/
 axios.get('/veu/bugs').then(
                response => {
                    console.log('服务器响应回来的数据',response.data)
                },
                error => {
                     console.log('错误信息', error.message)
                }
            )
~~~

原理：访问地址是 http://localhost:8080/bugs，会优先去 8080 服务器上找 /bugs 资源，如果没有找到才会走代理,通过脚手架的代理机制，访问 8000 服务器上的  /bugs 

注意：这种简单配置不支持配置多个代理。

##### 2.高级开启

支持配置多个代理

1.在vue.config.js 中配置：

~~~javascript
 //高级开启
  devServer: {
    proxy: {
      //凡是请求路径以 “/api” 开始的都走代理，
      '/api': {
        target: 'http://localhostP:8000',
        pathRewrite:{'^/api':''},
        ws: true,			//开启对websocket的支持（websocket是一种实时推送技术，默认不写，就是true
        chengeOrigin:true	// 默认值也是true，表示改变起源（让对方服务器不知道我们真的的起源到底是哪里）
      },
      '/abc': {
        target: 'http://localhostP:8001',
        pathRewrite: { '^/abc': '' },
        ws: true,
        chengeOrigin: true
      }
    }
  }

~~~

2.进行ajax 请求

~~~javascript
axios.get('/api/vue/bus').then(
                response => {
                    console.log('服务器响应回来的数据',response.data)
                },
                error => {
                     console.log('错误信息', error.message)
                }
            )
~~~

说明：

> 本服务器想访问 http://localhostP:8000/vue/bus  只需要在发送ajax请求出：axios.get('/api/vue/bus').then   凡是以‘/ api ’ 开头的，都会访问 http://localhostP:8000 上的资源，但是，如果不设置 pathRewrite 的话，vue会自动把api追加到url后面（ http://localhostP:8000/api/vue/bus），实际上 8000 服务器上并没有api这个路径，所以会导致出错。

**pathRewrite** 的原理：

>  pathRewrite 中以键值对存在，键前需要加上 ' ^ '  符号，{' ^/api ': ' ' } 原理是将 /api 转换为空的字符串，再进行拼接，最后拼接成为：http://localhostP:8000/vue/bus



#### 6.4**Vue** **插件库** **vue-resource** **发送** **AJAX** **请求**

1. 安装: npm i vue-resource
2. 导入：import vueResource from ‘vue-resource’
3. 使用插件：Vue.use(vueResource)
4. 使用该插件之后，项目中所有的 vm 和 vc 实例上都添加了：$http 属性。
5. 使用办法：(1) this.$http.get(‘’).then() 用法和 axios 相同，只不过把 axios 替换成 this.$http

在mian.js中使用插件

~~~javascript
//导入插件vue-resource
import VueResource from 'vue-resource'

//使用插件
Vue.use(VueResource)
~~~

在需要发送ajax请求处：

~~~javascript
this.$http.get('/vue/bus').then(
                response => {
                    console.log('服务器响应回来的数据', response.data)
                },
                error => {
                    console.log('错误信息', error.message)
                }
            )
~~~

### 7.Vuex 

#### 7.1 Vuex 概述

​		vuex 是实现 <u>数据集中式状态管理</u> 的插件。数据由 vuex 统一管理。其它组件都去使用 vuex 中的数据。只要有其中一个组件去修改了这个共享的数据，其它组件会同步更新。一定要注意：全局事件总线和 vuex 插件的区别：

> 1.全局事件总线关注点：组件和组件之间数据如何传递，一个绑定$on，一个触发$emit。数据实际上还是在局部的组件当中，并没有真正的让数据共享。只是数据传来传去。
>
> ![image-20230724164513770](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230724164513770.png)
>
> 2.vuex 插件的关注点：共享数据本身就在 vuex 上。其中任何一个组件去操作这个数据，其它组件都会同步更新。是真正意义的数据共享。
>
> ![image-20230724165724508](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230724165724508.png)

使用场景：	多个组件之间依赖于同一状态。来自不同组件的行为需要变更同一状态。

#### 7.2 **vuex** **环境搭建**

1. 安装 vuex

> (1) vue2 安装 vuex3 版本
>
>  		npm i vuex@3
>
> (2) vue3 安装 vuex4 版本
>
>  		npm i vuex@4

2. 在src目录下创建vuex目录和 js 文件（目录和文件名不是必须叫这个）

> (1) 目录：vuex
>
> (2) js 文件：store.js

3. 在 store.js 文件中创建核心 store 对象，并暴露

> store对象是vuex插件中的老大，最核心的对象，这个对象store是用来管理actions对象、mutations对象、state对象
>
> ~~~javascript
> //引入Vue，因为下面使用Vuex插件的时候需要vue
> import Vue from 'vue'
> //引入Vuex插件
> import Vuex from 'vuex'
> 
> //使用插件
> Vue.use(Vuex)
> //创建三个vuex插件的核心对象
> //创建store对象：actions对象、mutations对象、state对象
> const actions = {}
> const mutations={}
> const state = {}
> 
> //普通写法
>  const store = new Vuex.Store({
>  //他是一个负责执行某个行为的对象
>  actions: actions,
>  //他是一个负责更新的对象
>  mutations: mutations,
>  //它是一个状态对象
>  state: state
> })
> //导出store对象（暴露之后，别人想用可以使用import进行引入
> export default store 
> 
> /*
> //简写形式
> export default new Vuex.Store({actions,mutations,state})
> */

  4.在 main.js 文件中关联 store，这一步很重要，完成这一步之后，所有的 vm 和 vc 对象上会多一个$store 属性

> ~~~javascript
> //引入Vuex插件中的核心对象store
> import store from './vuex/store'
> 
> new Vue({
>   render: h => h(App),
>   //加上这个配置项之后，vm以及所有的vc对象上都会多一个属性：$store
>   store:store
> }).$mount('#app')
> ~~~
>
> 以后通过vm.$store 或者 vc.$store 获取store对象 

#### 7.3 简单使用Vuex

在store.js中 声明的state对象就相当于Vue中的data，用来存储数据的

1.在store中定义 num 属性，默认值为0

~~~javascript
const state = {
    num: 0
}
~~~

2.组件中访问num

~~~java
//html中访问
 <h1>数字：{{ $store.state.num }}</h1>
 //javascript中访问
this.$store.state.num
~~~

​	想要让num的值自增，可以这样做：this.$store.state.num++ 但是如果后期对num的值操作较大，并且很有可能需要代码的复用时，这样是不行的

-------

在Vuex中的三个核心对象：

> 1. actions
>
>    1. 它是一个负责执行某个行为的对象 
>
>    2. 其中包含多个action，每个action都是一个callback（回调函数）
>
>    3. action 是专门用来处理业务逻辑，或者专门用来发送ajax请求的
>
>    4. 使用：
>
>       ~~~javascript
>       //在actions对象中，定义名为plusOne的函数
>       const actions = {
>           	//context 参数：是vuex的上下文，可以看作store的压缩版
>               //value 参数：是传过来的数据
>           plusOne: function (context，value) { 
>               //对value进行操作
>               value=value+1
>               //业务逻辑处理完成之后，需要向下一个环节走，就轮到了数据更新
>               //提交上下文环境
>               context.commit('mutation'，value)
>           }
>       }
>       ~~~
>
>       利用Vuex的API（dispatch 分发），调用这个方法之后，store对象中的plusOne这个action 回调函数会被自动调用
>
>       语法：
>
>       ​			this.$store.dispatch(' 需要执行的回调函数 ', 向回调函数中传递的参数)
>
>       ~~~javascript
>             this.$store.dispatch("plusOne",this.starNUm)
>
> 2. mutations
>
>    1. 它是一个负责更新的对象
>
>    2. 其中包含多个mutation，每个mutation都是一个callback（回调函数）,它是用来更新state中的数据的，只要state一更新，因为是响应式的，所有页面就重新渲染了。
>
>    3. 使用：
>
>       等到 actions 中处理完业务逻辑之后，就将任务交给 mutation ，一般来讲，在mutations中的函数名都采用大写的形式。
>
>       ~~~javascript
>       const mutations = {
>           //state:就是状态对象
>           //value ：就是上个环节传递过来的数据
>           PLUS_ONE(state, value) {
>               state.num += value
>           }
>       }
>
> 3. state
>
>    1. 它是一个状态对象，类似于Vue中的data，用来存储数据的

思路：

> 在html中点击按钮，触发事件，调用回调函数，在函数体中，执行：this.$store.dispatch("plusOne",this.starNUm) ，dispatch方法自动调用 vuex 中 actions 对象中定义好的 plusOne 函数，并向其传递参数（ this.starNUm ），plusOne 中的value参数用来接收传递过来的参数，在 plusOne  函数体中处理数据，最后通过 plusOne  的context.commit参数提交任务并且传递处理好的数据value给 mutations 中的PLUS_ONE 函数，最后更新 state 中的数据

注意：如果想 action 调用另外的 action 可以使用dispatch,但是需要content调用dispatch方法

~~~javascript
const actions = {
    plusOne: function (context, value) {
        value = value + 1
        context.dispatch('otherAction', value)
        context.commit('PLUS_ONE', value)
    },
    otherAction(context, value) {
        console.log(value)
    }
}
~~~

#### 7.4 工作原理

![image-20230725165533362](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230725165533362.png)

#### 7.5 越过 action 直接 commit

如果action中的操作比较简单，比如只把state中的number加一，可以越过actions，直接通过commit 到mutations ，不再通过actions

触发事件后：

~~~javascript
  methods: {
    plusOnes() {
     //不再通过dispatch转发到actions，而是直接转发到mutations
      //this.$store.dispatch("plusOne", this.startNum)
      this.$store.commit('PLUS_ONE',this.startNum)
    }
  }
~~~

在mutations中处理数据即可

#### 7.6 vuex 的 getters 配置项

作用：类似于计算属性

getters 中有很多getter，每个getter可以看作计算属性，并且它会接受一个参数，这个参数就是state，这样可以对state中的属性进行操作

~~~javascript
//全新的配置项
const getters = {
    //其中有很多getter，每一个getter都可以看作一个计算属性
    //每一个getter方法都会自动接受一个state对象
    
    reversedName(state) {//可以看作vue中的computed
        return state.num+1  //让state中的num数字加一并返回，但是并不会修改state中的值
    }
}
~~~

向vuex中添加配置项

~~~javascript
export default new Vuex.Store({ actions, mutations, state ,getters})
~~~

使用getters中的值

~~~html
    <p>{{ $store.getters.reversedName }}</p>
~~~

#### 7.7 **mapState** 和 **mapGetters** 的使用（优化计算属性）

##### 1.回顾ES6中的扩展运算符

  扩展运算符可以作用到数组中，也能作用到对象上，其主要作用是取出数组中的所有元素，以及对象中的所有属性

针对数组

> ~~~html
> <script>
>         const array = [1, 2, 3, 4, 5, 6]
>         console.log(...array)  //输出为： 1,2,3,4,5,6
>     </script>

针对对象

> ​	扩展运算符能对对象进行操作，但是不能直接输出，其原理是将对象的大括号去掉，暴露出其内部的属性，但是不能直接输出这些属性，可以对这些对象再加上一个大括号
>
> ~~~html
>     <script>
>         const o1 = {
>             x: 1,
>             y: 1
>         }
>         // console.log(...o1)
>         console.log({ ...o1 })  //输出为:{x:1,y:1}
>         const o2 = { ...o1 }
>         console.log(o2 === o1)  //输出为：false
>     </script>
> ~~~
>
> 注意：通过扩展运算符操作完对象后，其原本的对象与现在的对象，不是同一个对象，即 o1 与 o2 是两个不同的对象。

扩展

> 使用扩展运算符，将多个对象进行拼接
>
> ~~~html
>     <script>
>         const o3 = {
>             a: 1,
>             b: 2
>         }
>         const o4 = {
>             c: 3,
>             d: 4
>         }
>         const o5 = {
>             e: 5,
>             f: 6
>         }
>         console.log({ x: 2, ...o3, ...o4, y: 3 })  //{x: 2, a: 1, b: 2, c: 3, d: 4, …}
>         const o6 = { ...o3, ...o4, ...o5 }  
>         console.log(o6)//{a: 1, b: 2, c: 3, d: 4, e: 5, …}
> 
>     </script>

##### 2.前景

组件中在使用state上的数据时，有固定到前缀：

> ~~~html
> <p>{{$store.state.users}}</p>
> ~~~

使用起来非常的不方便，可以通过计算属性进行优化：

> ~~~javascript
> computed: {
>         user() {
>             return this.$store.state.users
>         },
>         vips() {
>             return this.$store.state.vips
>         },
>          reverseStr(){
>              return this.$stroe.getters.reverseStr
>          }
>     },
> ~~~

> ~~~html
>         <p>用户人数：{{ users.length }}</p>
>         <p>vip人数：{{ vips.length }}</p>
> 
> ~~~

##### 3.mapState与mapGetters

​	由于定义多个计算属性会非常的麻烦，vuex插件提供了一个内置的mapState对象和mapGetters对象。

​	**注意**：mapState相当于是一个函数，它用来代替我们去声明许多的计算属性。mapState生成的对象中包含一个个函数，相当于计算属性。但是其生成的计算属性是简写形式，只有get方法，没有set方法，对于双向数据绑定来讲，程序会报错。

> 1.在使用mapState前需要进行导入
>
> ~~~javascript
> import {mapState，mapGetters} from 'vuex' 
> ~~~
>
> 2.在mounted函数中使用
>
> 第一种：对象形式
>
> ​	对象形式中采用键值对的形式存在，key相当于计算属性的名字，value就是state中的值
>
> ~~~javascript
>     computed: {
>         //这是以前的方式进行简化代码
>        /*  user() {
>             return this.$store.state.users
>         },
>         vips() {
>             return this.$store.state.vips
>         } */
>         //通过mapState，可以代替以上的操作，它生成一个对象，通过扩展运算符将对象中的属性取出来
>         ...mapState({users:'users',vips:'vips'})
>         
>         ...mapGetters({reverseStr:'reverseStr'})
>     },
> ~~~
>
> 第二种：数组形式：
>
> ​	这种方式需要state中的属性名与计算属性的属性名保持一致
>
> ~~~7.javascript
>     computed: {
>         // 数组形式
>          ...mapState([ 'users', 'vips' ])
>          ...mapGetters([{ 'reverseStr' }])
>     },

#### 7.8 **mapMutations** 和 **mapActions** 的使用（优化methods）

~~~javascript
 methods: {
        add() {
             this.$store.dispatch('adduesr', this.newUser)
            this.$store.commit('ADD_VIP', this.newVip)
        }
    }
~~~

传统使用vuex进行分发任务时，需要使用到dispatch或者commit，向 actions 或 mutations 分发任务并传递数据，函数体比较单一。

使用 mapMutations 和mapActions 可以简化这一操作。mapMutations 和mapActions 可以帮助我们自动生成一个函数。

> 1.引入
>
> ~~~javascript
> import { mapActions,mapMutations } from 'vuex'
> ~~~
>
> 2.在methods中使用
>
> 它以键值对的形式存在，key表示函数名，value表示store中actions或mutations中的函数名
>
> ~~~javascript
>     methods: {
>        /*  add() {
>             this.$store.commit('ADD_USER', this.newUser)
>         } */
>         //使用以下代码能帮助我们自动生成以上代码，并且传递参数
>         //对象形式
>         ...mapActions({ addUser :'addUser'})
>         
>         ...mapMutations({ ADD_USER :'ADD_USER'})
>         
>         //数组形式（需要两个方法名一致
>         ...mapActions(['addUser'])
>         
>         ...mapMutations(['ADD_USER'])
>     }
> ~~~
>
> 3.传递参数：
>
> ~~~html
>         <button @click="ADD_USER(newUser)">添加</button>

#### 7.9 Vuex的模块化开发

##### 1.在store.js文件中新建多个模块

> 1.新建a、b两个对象，对象中有actions、mutations、state、getters等属性。最后在 new Vuex.$Store({}) 中使用modules属性添加模块
>
> ~~~javascript
> //新建a对象
> const a = {
>     actions: {
>         anum() {
>             return 1
>         }
>     },
>     mutations: {
>         ANUM() {
>             return 1
>         }
>     },
>     state: {
>         a:1
>     },
>     getters: {
>         gnum() {
>             return 1
>         }
>     }
> }
> //新建b对象
> const b = {
>     actions: {
>         bnum() {
>             return 1
>         }
>     },
>     mutations: {
>         BNUM() {
>             return 1
>         }
>     },
>     state: {
>         b: 1
>     },
>     getters: {
>         gnum() {
>             return 1
>         }
>     }
> }
> export default new Vuex.Store({
>     modules: {
>         //模块名:对象
>         aModule: a,
>         bModule:b
>      }
>  })
> ~~~
>
> 2.在组件中使用模块
>
> > - 使用state属性中的值
> >
> > ~~~html
> > <!--使用a模块state中的属性-->
> > <P>{{$store.state.aModule.a}}</P>
> > <!--使用b模块state中的属性-->
> > <P>{{$store.state.bModule.b}}</P>
> > ~~~
> >
> > - 使用 actions 和 mutations 中的函数
> >
> > ~~~javascript
> >     /*使用一下的方法访问模块中的action或mutation存在缺点：例如使用dispatch分发任务时，vuex会先在a模块中寻找anum函数，如果a没有则去b模块中寻找，直到找到为止，但如果a模块和b模块中都含有anum，则两个函数都会运行。
> >     */
> > 	methods: {
> >         anum() {
> >             this.$store.dispatch('anum')
> >              this.$store.commit('ANUM')
> >         }
> >     }
> > ~~~
> >
> > //解决方法：启用命名空间，将所有的模块都添加一个配置项（namespaced），默认值为false。
> >
> > ~~~javascript
> > 
> > //新建a对象
> > const aModule = {
> >     namespaced:true,
> >     actions: {
> >         anum() {
> >             console.log('a中action执行了')
> >         }
> >     },
> >     mutations: {
> >         ANUM() {
> >             console.log('a中mutation执行了')
> >         }
> >     },
> >     state: {
> >         a: 'a模块中的数据'
> >     },
> >     getters: {
> >         ganum() {
> >             return 'a中getter'
> >         }
> >     }
> > }
> > ~~~
> >
> > 使用a模块中的action、mutation
> >
> > ~~~javascript
> >         anum() {
> >             this.$store.dispatch('aModule/anum')
> >         },
> >         ANUM() {
> >            this.$store.commit('amodule/ANUM') 
> >         },
> > ~~~
> >
> > 使用模块中的getter
> >
> > ~~~html
> >         <p>a模块中的getters中的属性::{{ $store.getters['aModule/ganum'] }}</p>
> >         <p>a模块中的getters中的属性::{{ $store.getters['bModule/gbnum'] }}</p>
> > ~~~

2.新建多个js文件，每个文件对应一个模块

> 新建aModule.js和bModule.js 文件，在文件中写入代码并导出（ export default）
>
> 在store.js文件中导入每个模块，并使用modules配置项加载到vuex中
>
> ~~~javascript
> import aModule from './aModule'
> import bModule from './bModule'
> 
> export default new Vuex.Store({
>     modules: {
>         aModule, bModule
>     }
> })

3.面对mapXxxx使用命名空间

> ~~~javascript
>     computed: {
>         ...mapState('aModule', ['a']),
>         ...mapState('bModule', ['b']),
>         ...mapGetters('aModule', ['ganum']),
>         ...mapGetters('bModule', ['gbnum']),
>     },
>     methods: {
>         ...mapActions('aModule', ['anum']),
>         ...mapActions('bModule', ['bnum']),
>         ...mapMutations('aModule', ['ANUM']),
>         ...mapMutations('bModule', ['BNUM'])
> 
>     }

### 8.路由 route

​	路由器（router）是用来管理或调度各个路由（route）的。对于一个应用来说，一般路由器只需要一个，但是路由是有多个

#### 8.1 传统的web应用 vs 单页面web应用

**传统 web 应用**

> 传统 web 应用，又叫做多页面 web 应用：核心是一个 web 站点由多个 HTML 页面组成，点击时完成页面的切换，因为是切换到新的 HTML 页面上，所以当前页面会全部刷新

**单页面web 应用（SPA：Single Page web Application）**

> 整个网站只有一个 HTM 页面，点击时只是完成当前页面中**组件**的切换。属于页面局部刷新。
>
> 单页应用程序 (SPA) 是加载单个 HTML 页面并在用户与应用程序交互时动态更新该页面的 Web 应用程序。浏览器一开始会加载必需的 HTML、CSS 和 JavaScript，所有的操作都在这张页面上完成，都由 JavaScript 来控制。单页面的跳转**仅刷新局部资源**。因此，对单页应用来说**模块化的开发和设计**显得相当重要。 

**单页面与多页面之间的对比**

![image-20230812101936572](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230812101936572.png)

#### 8.2 路由器与路由的工作原理

路由器会一直监听url，如果路径发生变化，路由器会根据路径匹配相应的组件，并加载，实现一个html网页多个组件的功能 

![image-20230812102449884](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230812102449884.png)

#### 8.3 使用路由

第一步：安装 vue-router 插件

> (1) vue2 要安装 vue-router3
>
> ​	 npm i vue-router@3
>
> (2) vu3 要安装 vue-router4
>
> ​	 npm i vue-router@4

第二步：在main.js文件中导入插件并使用

> ~~~javascript
> //导入插件vue-router
> import VueRouter from 'vue-router'
> //使用vue-router插件
> Vue.use(VueRouter)

第三步：在src下新建一个文件夹router，在该文件夹下新建一个index.js文件，在该文件中定义路由器对象

> path配置项对应的是路由器需要检测的路径，需要以'/ ' 开始，component配置项对应的是组件对象
>
> ~~~javascript
> //导入vue-router插件
> import VueRouter from "vue-router";
> //导入组件
> import sc from '../components/sc.vue'
> import cq from '../components/cq.vue'
> //创建路由器对象（在路由器对象中配置路由）
> const router = new VueRouter({
>     //在这里配置所有的路由规则
>     routes: [
>         //这就是一个路由
>         {
>             //这个可以看作是路由的key
>             path: '/sc.html',
>             //这个可以看作是路由的value，路由的value可以看作是一个组件。
>             component: sc
>         },
>         {
>             path: '/cq.html',
>             component: cq
>         }
>     ]
> 
> })
> //暴露路由器对象（导出路由器对象）
> export default router

第四步：在main.js中导入路由器对象文件，并通过vue的router配置项进行配置

> ~~~javascript
> //导入路由对象
> /*如果是index.js文件，则可以直接导入router路径
> 	import router from './router'
> */
> import router from './router/index.js'
> 
> new Vue({
>   render: h => h(App),
>     //router配置项，对应的是路由器对象
>   router: router
> }).$mount('#app')

第五步:在app中使用路由

> 如果是使用路由方式，就不能使用a标签去访问新的路劲了，而是使用vue-router插件提供的标签：router-link。这个标签在将来会被自动编译为a标签。
>
> vue-router插件还提供了另外一个标签：router-view。该标签就是起到一个站位符的作用，将来需要加载的组件会加载到这个标签中去。
>
> 注意：router-link标签中的to属性类似于a标签中的href，但是其值是以'/'开头以路由中的path值结尾
>
> ~~~html
>     <div>
>         <div>
>             <ul>
>                 <li><router-link to="/sc">四川</router-link></li>
>                 <li><router-link to="/cq">重庆</router-link></li>
>             </ul>
>         </div>
>         <!-- 在这里显示对应的组件 -->
>         <router-view></router-view>
>     </div>

#### 8.4多级路由

在一个路由中可以嵌套多个路由，需要使用到 children 配置项。

> ~~~javascript
> const router = new VueRouter({
>     //在这里配置所有的路由规则
>     routes: [
>         //这就是一个路由
>         {
>             //这个可以看作是路由的key
>             path: '/sc',
>             //这个可以看作是路由的value，路由的value可以看作是一个组件。
>             component: sc,
>             //children配置项，对象是数组形式。
>             children: [
>                 //这就是一个子路由
>                 {
>                     path: 'meishan',
>                     component: meishan
>                 },
>                 //这也是个子路由
>                 {
>                     path: 'luzhou',
>                     component: luzhou
>                 }
>             ]
>         }
>     ]
> 
> })
> ~~~

使用子路由

> ~~~html
>             <ul>
>                 <li><router-link to="/sc/meishan" active-class="astyle">眉山</router-link></li>
>                 <li><router-link to="/sc/luzhou" active-class="astyle"  >泸州</router-link></li>
>                 <li>宜宾</li>
>             </ul>
> <router-view class="xian"></router-view>
> ~~~

注意：

> 1.子路由中的path配置项，其路劲无需以 '/' 开头， vue会自动为其添加
>
> 2.在router-link标签中的 to 属性，需要带上父路由的路径
>
> 3.一般情况下，路由组件会放置在pages的文件中，而普通组件才放在components中

#### 8.5 路由 query 传参

<u>对于每一个子路由对象，都有一个$route参数，其中有个query参数，就是用来存储父路由给子路由传递的参数。</u>

父路由向子路由传递参数

> 1.使用query传参，采用字符串形式
>
> ~~~html
> <li><router-link to="/sc/city?c1=城市1&c2=城市2&c3=城市3" active-class="astyle">眉山</router-link></li>
> ~~~
>
> 使用es6语法，实现动态字符串拼接：
>
> ~~~html
> <li><router-link :to="`/sc/city?c1=${citys.meishan[0]}&c2=${citys.meishan[1]}&c3=${citys.meishan[2]}`"
>                         active-class="astyle">眉山</router-link></li>
> ~~~
>
> 2.使用query传参，采用对象形式
>
> ~~~html
>                 <li><router-link :to="{
>                     path: '/sc/city',
>                     query: {
>                         c1: citys.meishan[0],
>                         c2: citys.meishan[1],
>                         c3: citys.meishan[2],
>                     }
>                 }" active-class="astyle">眉山</router-link></li>

子路由接受参数并使用

> ~~~html
>         <ul>
>             <li v-for="(cityname, index) of $route.query" :key="index">{{ cityname }}</li>
>         </ul>
> ~~~

$route对象

> ![image-20230812160203385](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230812160203385.png)

$toute.query对象

> ![image-20230812160244438](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230812160244438.png)

#### 8.6 给路由命名

当在路由中有多级路由时，path的路径名会很长，为了避免这种情况发生，可以给路由起名。只需要在路由对象中添加name属性，在标签中使用name，但是需要注意的是，想要使用路由名进行访问，router-link标签中的to属性，必须是 v-bind：to

1.给路由起名

~~~javascript
routes: [
        //这就是一个路由
        {
            //这个可以看作是路由的key
            path: '/sc',
            //这个可以看作是路由的value，路由的value可以看作是一个组件。
            component: sc,
            children: [
                {
                    //给路由起名
                    name: 'mei',
                    path: 'meishan',
                    component: city
                },
                {
                    name: 'lu',
                    path: 'luzhou',
                    component: city
                },
            ]
        }
    ]
~~~

2.使用

~~~html
                <li><router-link :to="{
                    name: 'lu',
                    //path: '/sc/luzhou',
                    query: {
                        c1: citys.luzhou[0],
                        c2: citys.luzhou[1],
                        c3: citys.luzhou[2],
                    }
                }" active-class="astyle">泸州</router-link></li>
~~~

#### 8.7 路由params传参

在$route对象中还存在着与query相似的属性--params属性，用法和query类似。

1.标注路径

> ~~~javascript
>                 {
>                     name: 'lu',
>                     path: 'luzhou/:c1/:c2/:c3',
>                     component: city
>                 },
> ~~~
>
> 以':'开头，表示将来是传递的参数

2.传递参数

> 字符串形式传递参数
>
> ~~~html
> <li><router-link to="/sc/luzhou/县1/县2/县3">泸州</router-link></li>
> <li><router-link :to="`/sc/luzhou/${city.luzhou[0]}/${city.luzhou[1]}/${city.luzhou[2]}`">泸州</router-link></li>
> ~~~
>
> 对象形式传递参数
>
> 注意：使用对象形式传递参数，必须使用name，不能使用path
>
> ~~~html
>                 <li><router-link :to="{
>                     name: 'lu',
>                     params: {
>                         c1: citys.luzhou[0],
>                         c2: citys.luzhou[1],
>                         c3: citys.luzhou[2]
>                     }
>                 }">泸州
>                     </router-link>
>                 </li>

3.接收参数

~~~html
        <ul>
            <li v-for="(cityname, index) of $route.params" :key="index">{{ cityname }}</li>
        </ul>
~~~

#### 8.8 路由的props

props 配置主要是为了简化 query 和 params 参数的接收。让插值语法更加简洁。

第一种方式：（props为对象形式）

> 在路由对象中添加props对象
>
> ![image-20230812170001144](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230812170001144.png)
>
> 在路由组件中接收参数并使用
>
> ![image-20230812170153160](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230812170153160.png)

第二种方式：（props为函数形式）

> 当props为函数时，route会为其自动传递一个参数，该参数就是这个子路由对象中的 $route
>
> ![image-20230812170638411](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230812170638411.png)
>
> ![image-20230812170622615](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230812170622615.png)

第三种实现方式：直接将 params 方式收到的数据转化为 props

> 注意：这种方式，只支持params传参的时候使用，	是直接讲params方法接收的到的参数直接传递给props，是内部进行的
>
> ![image-20230812171135002](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230812171135002.png)
>
> ![image-20230812171111601](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230812171111601.png)

#### 8.9 router-link 的 replace属性

了解浏览器的前进和回退

> 1.浏览器的历史记录是存放在栈这种数据结构当中的。
>
> 2.历史记录在存放的栈这种数据结构的时候有两种不同的模式：
>
> ​	第一种：push模式
>
> ​		以追加的方式入栈。
>
> ​	第二种：replace模式
>
> ​		以替换栈顶元素的方式入栈。
>
> 3.浏览器默认的模式是：push模式。
>
> ​	操作浏览器上的前进和后退的时候，并不会删除栈中的历史记录，后退是向下移动指针，前进是向上移动指针。

开启replace模式

~~~html
<router-link :replace="true"> </router-link>
<!--或者：简写形式-->
<router-link replace> </router-link>
~~~

#### 8.10 编程式路由导航

在之前的学习当中，想要组件的切换使用的是声明式路由导航，即声明一个router-link标签，在标签中添加请求路径。而编程式路由导航指的是，通过一个函数来切换路由组件。（点击一个按钮，就切换路由组件，不再使用超链接的形式）

在子路由中，this.$route是代表的路由对象，而还存在一个属性$router，代表的是路由器对象，一个项目中最好只有一个路由器对象。

------

使用编程式路由导航

> 使用push模式
>
> ~~~javascript
> mcity() {
>             this.$router.push({
>                 name: 'mei',
>                 params: {
>                     c1: this.citys.meishan[0],
>                     c2: this.citys.meishan[1],
>                     c3: this.citys.meishan[2]
>                 }
>             })
>         }
> ~~~
>
> 使用replace模式：
>
> ~~~javascript
> lcity() {
>             this.$router.replace({
>                 name: 'lu',
>                 params: {
>                     c1: this.citys.luzhou[0],
>                     c2: this.citys.luzhou[1],
>                     c3: this.citys.luzhou[2]
>                 }
>             })
>         }
> ~~~
>
> 前进
>
> ~~~javascript
> forward(){
> 	this.$router.forward()
> }
> ~~~
>
> 后退
>
> ~~~javascript
> back(){
>     this.$router.back()
> }
> ~~~
>
> 前进几步
>
> ~~~javascript
> goto(){
>     this.$router.go(2)//前进两步
> }
> ~~~
>
> 后退几步
>
> ~~~javascript
> toback(){
> 	this.$router.go(-2)//后退两步
> }

注意：使用编程式路由导航的时候，需要注意：重复执行 push 或者 replace 的 API 时，会出现以下错误：

![image-20230812184501762](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230812184501762.png)

在使用编程式的路由导航时，push以及replace方法会返回一个Promise对象

Promise对象期望你能通过参数的方式给它两个回调函数，一个是成功的回调函数，一个是失败的回调函数，如果没有给这两个回调函数，则会出现以上错误。

解决：在参数末尾给两个回调函数

![image-20230812185023241](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230812185023241.png)

#### 8.11缓存路由组件

​	将a路由组件切换到b路由组件时，a路由组件就会被销毁掉，更换成b路由组件，这意味着a组件中原有的状态都会消失（例如复选框选中），为了避免这一现象发生可以添加keep-alive标签,使用这个标签将router-view标签包裹后，当切换到其他组件时，以前的组件不会被销毁掉

指定所有组件都不会被销毁

> ~~~html
> <keep-alive>
>     <router-view></router-view>
> </keep-alive>
> ~~~

指定某个组件的状态不会销毁----include

> ~~~html
> <keep-alive :include='组件名称'>
>     <router-view></router-view>
> </keep-alive>
> ~~~
>
> 这里的组件名称指的是：
>
> ![image-20230812190254419](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230812190254419.png)

指定多个组件不会被销毁

> ~~~html
> <keep-alive :include=['组件名1','组件名2']>
>     <router-view></router-view>
> </keep-alive>

#### 8.12 activated 和  deactivated

对于普通组件来说，生命周期当中具有9个钩子函数：八个基础的钩子函数+this.nextTick( function(){} ), nextTick 的作用是在下一次DOM渲染的时候钩子执行。

对于路由组件来说，除了9个钩子函数以外，还有两个：

1. activated：在路由组件被激活的时候执行
2. deactivated：在路由组件被切走的时候执行

#### 8.13 路由守卫

类似于javaWeb中的filter 过滤器，在执行某段代码之前执行。例如在使用某些功能时，判断用户是否登录，或者权限是否足够，如果没有登录或访问权限不够，则不能使用该功能。

##### 8.13.1全局前置守卫

​	1.位置：在创建好router对象之后，在暴露router对象之前。

​	2.语法： router.beforeEach( 回调函数callback )

​	3.callba函数的调用时间：beforeEach中的callback在初始化的时候被调用一次，以及在每一次切换任意组件之前被自动调用。

​	4.callback有三个参数：from、to、next

> ​	from参数：from是一个路由对象，表示切换路由组件之前的路由对象
>
> ​	to参数：to也是一个路由对象，表示需要切换到新的路由组件的路由对象
>
> ​	next参数：这是一个函数，调用这个函数之后，表示放行，可以继续往下走

![image-20230812224026413](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230812224026413.png)

![image-20230812224040617](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230812224040617.png)

to、from、next的运用：

> to和from都是路由对象，其中有path/name属性,可以通过他们来判断是否放行。

> ~~~javascript
> router.beforeEach((to, from, next) => {
>     if (from.path === '/sc') {
>         if (to.name === 'meishan') {
>             next()
>         }
>     }
> })
> ~~~

在大多数情况下，只有一部分路由组件时需要被拦截并做判断的，此时在beforeEach的回调函数中对所有组件做判断会很麻烦。

> ​		解决：给需要被拦截判断的组件添加一个属性，该属性的值是布尔类型的。对于需要被拦截判断的组件，设置该值为true，则表示需要被拦截判断，对于不需要被拦截判断的组件，不需要为其添加属性，因为在访问这个路由对象的该属性时，如果该路由对象没有该属性，则为undefined，undefined后期会变成false。

注意：想要给路由对象添加自定义属性，必须在路由对象的 meta（路由源）中定义。

1.定义自定义属性

> ~~~javascript
> 			{
>                     name: 'mei',
>                     path: 'meishan/:c1/:c2/:c3',
>                     component: city,
>                     props: true,
>                     //自定义属性
>                     meta: {
>                         isAuth: true
>                     }
>                     },
> ~~~

2.通过自定义属性判断是否需要被拦截

> ~~~javascript
> router.beforeEach((to, from, next) => {
>     if (to.meta.isAuth) {
>         next()
>     }
> })
> ~~~

##### 8.13.2 全局后置守卫

​	1.位置：在创建好router对象之后，在暴露router对象之前。

​	2.语法： router.afterEach( 回调函数callback )

​	3.callba函数的调用时间：afterEach中的callback在初始化的时候被调用一次，以及在每一次切换任意组件之后被自动调用。

​	4.callback有三个参数：from、to

~~~
router.afterEach((to, from) => {
    console.log(to)
    console.log(from)
    document.title = to.meta.title || '欢迎使用本网站' //如果meta.title为undefined时，执行：document.title='欢迎使用本网站'
})
~~~

##### 8.13.3 局部路由守卫之path守卫

​	1.位置：写在route对象中

​	2.语法： beforeEnter( to, from, next){ }

​	3.beforeEnter() 本身就是一个函数，他的参数不再是回调函数， 它也有三个参数：to, from, next

> ​	from参数：from是一个路由对象，表示切换路由组件之前的路由对象 
>
> ​	to参数：to也是一个路由对象，表示需要切换到新的路由组件的路由对象
>
> ​	next参数：这是一个函数，调用这个函数之后，表示放行，可以继续往下走

4.beforeEnter()函数在进入指定组件之前被触发。

![image-20230814104314560](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230814104314560.png)

##### 8.13.4 局部路由守卫之component守卫

位置：写在以.vue结尾的组件中。

语法：

> 1.beforeRouterEnter( to, from ,next ) {}     ————进入路由组件之前执行
>
> 2.beforeRouterLeave( to, from, next ){}     —————离开路由组件之前执行

注意：普通组件不会触发，路由组件才会触发，也就是只有配置到路由器对象中的组件才会触发

#### 8.14 前端项目上线

​		源代码Xxx.vue对于浏览器来说，浏览器只认识html、css、js代码。Xxxx.vue需要使用项目构建工具完成打包编译。例如，可以使用webpack来打包编译。生成html、css、js代码，让浏览器识别。

------

1.路径中#后面的路径称为 hash。这个 hash 不会作为路径的一部分发送给服务器：

> ​	http://localhost:8080/vue/bugs/#/a/b/c/d/e （真实请求的路径是：http://localhost:8080/vue/bugs）

2.路由的两种路径模式：

> hash模式
>
> history模式

3.默认是 hash 模式，如何开启 history 模式：

> router/index.js 文件中，在创建路由器对象 router 时添加一个 mode 配置项：
>
> ![image-20230814111042233](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230814111042233.png)

4.项目打包

> 使用命名：npm run build
>
> 讲项目打包，会生成一个dist文件夹，其中包含css文件夹、js文件夹、index.html
>
> ![image-20230814114534856](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230814114534856.png)

5.使用项目

> ​	将以上文件全部复制，粘贴到Tomcat文件夹下的webapps/ROOT 中（清空ROOT文件中的内容后再粘贴），启动Tomcat服务器后，就会加载 index.html 文件。
>
> 对于history模式来说,当刷新页面时，就会发现404错误。
>
> ​	原因：点击刷新页面,请求路径如果发生了变化,那么浏览器会向服务器提交申请,然而浏览器并没有相应的资源.
>
> ​	例:当请求路径从"http://localhost:8080" 变为 "http://localhost:8080/sc/meishan/仁寿/东坡区" 后刷新页面，就会出现404错误。
>
> 对于hash模式来说，就不会出现以上问题，浏览器不会把后面的参数作为请求路径发送到服务器，所以不会出现404错误。
>
> 即想使用history模式，又不想出现404 错误的解决办法：
>
> 1.在ROOT文件夹中添加名为 WEB-INF 的文件夹，其中存放一个名为web.xml的文件
>
> 2.配置web.xml文件：
>
> <img src="C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230814140336735.png" alt="image-20230814140336735" style="zoom: 67%;" />
>
> 含义：当发生404错误时，跳转到 index.html 页面中

6.hash 模式和 history 模式的区别与选择

> hash模式：
>
> > 1.路径中带有#，不美观。
> >
> > 2.兼容性好，低版本浏览器也能用。
> >
> > 3.项目上线刷新地址不会出现 404。
> >
> > 4.第三方 app 校验严格，可能会导致地址失效。
>
> history模式：
>
> > 路径中没有#，美观。
> >
> > 兼容性稍微差一些。
> >
> > 项目上线后，刷新地址的话会出现 404 问题。需要后端人员配合可以解决该问题

### 9.Vue3

#### 9.1 了解Vue3

------

1.vue3 做了哪些改动：

> (1) 最核心的虚拟 DOM 算法进行了重写。
>
> (2) 支持 tree shaking：在前端的性能优化中，es6 推出了 tree shaking 机制，tree shaking 就是当我们在项目中引入其他模块	 	  时，他会自动将我们用不到的代码，或者永远不会执行的代码摇掉
>
> (3) 最核心的响应式由 Object.defineProperty 修改为 Proxy 实现。
>
> (4) 更好的支持 TS（Type Script：TypeScript 是微软开发的一个开源的编程语言，通过在 JavaScript 的基础上添加静态类型定义	  构建而成。TypeScript 通过 TypeScript 编译器或 Babel 转译为 JavaScript 代码，可运行在任何浏览器，任何操作系统。）
>
> (5) 等。。。

2.Vue3 的新特性

> (1) 新的生命周期钩子
>
> <img src="C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230814162154774.png" alt="image-20230814162154774" style="zoom:50%;" />
>
> (2) 键盘事件不再支持 keyCode。例如：v-on:keyup.enter 支持，v-on:keyup.13 不支持。
>
> (3) 组合式 API（Composition API）
>
> <img src="C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230814162239991.png" alt="image-20230814162239991" style="zoom: 50%;" />
>
> （4）新增了一些内置组件
>
> <img src="C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230814162319462.png" alt="image-20230814162319462" style="zoom:50%;" />
>
> (5) data 必须是一个函数。
>
> (6) 等...

#### 9.2 创建Vue3工程



##### 9.2.1 vue-cli 创建 Vue3 工程

------

1.创建 Vue3 版本的工程，要求 vue-cli 最低版本 4.5.0

> 查看版本：vue --version
>
> 升级脚手架：npm install -g @vue-cli

2.创建 Vue3 工程

> 1.vue2工程是通过vue-cli脚手架完成创建的。vue-cli 脚手架是基于webpack项目构建工具实现的。
>
> 2.vue3工程也可以通过vue-cli创建。但目前更加流行的方式是另一个脚手架create-vue来完成vue3工程的创建。
>
> 3.create-vue这个脚手架是基于vite项目构建工具来完成的，（vite和webpack一样，都是项目构建工具）
>
> 4.vite的特点：
>
>  	(1)服务器启动速度非常快，然后代码修改之后，更新非常快。这是使用vite的主要原因。
>														
>  	(2)vite比webpack性能要好很多
>
> 5.使用vue-cli创建项目：vue create vue3_pro

3.启动工程

> (1) 切换到工程根目录。
>
> (2) npm run serve

##### 9.2.2 vue-cli 创建的Vue3 项目说明

1. 目录结构以及文件和 vue2 相同。

   <img src="C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230814164049137.png" alt="image-20230814164049137" style="zoom:67%;" />

2.  main.js 文件说明

   ~~~javascript
   //在vue3中，不再引入vuel，引入了一个createApp函数买这个函数可以创建app对象
   import { createApp } from 'vue'
   // 引入一个根组件App
   import App from './App.vue'
   /**
    * 这行代码表示创建一个app对象
    * 这个app对象类似于vue2当中的vm。
    * app和vm的区别：app更加轻量级。（app上的属性更少一些） 
    *  */
   const app = createApp(App)
   //将app挂载到指定位置
   app.mount('#app')
   /*
   三秒后卸载
   setTimeout(()=>{
   app.unmount('#app')
   },5000)
   */
   ~~~

3. 查看 App.vue 组件

   1. vue3 中 template 标签下可以有多个根标签了。不需要只有一个根标签
##### 9.2.3 create-vue 创建 Vue3 工程

------

 1.create-vue 是什么？

> (1) 和 vue-cli 一样，也是一个脚手架。
>
> (2) vue-cli 创建的是 webpack+vue 项目的脚手架工具。
>
> (3) create-vue 创建的是 vite+vue 项目的脚手架工具。
>
> (4) webpack 和 vite 都是前端的构建工具。

2. vite 官网

> (1) https://vitejs.cn/

3.安装create-vue 脚手架来创建vue3工程

> (1)指令：npm init vue@latest
>
> (2)安装create-vue脚手架，同时创建vue3工程。如果已经安装create-vue脚手架，还是采用这种方式创建vue3的工程 .
>
> (3)第一次检测到没有安装create_vue脚手架时，会提醒你安装：
>
> ![image-20230814174203995](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230814174203995.png)
>
> (4)安装好create-vue 脚手架之后，会提示设置项目的名字：
>
> ![image-20230814174705177](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230814174705177.png)
>
> (5)创建完项目之后还需要切换到项目目录下，使用npm install 命令安装，最后使用npm run dev 编译运行
>
> ![image-20230814174843173](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230814174843173.png)
>
> 

##### 9.2.4 create-vue 创建的vue3 工程说明

1.目录结构

![image-20230814175531643](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230814175531643.png)

2. 和vue-cli 脚手架创建的区别：

> （1）index.html 文件不再放在public目录下了
>
> ​	vite 以index.html 作为入口，不再用main.js作为入口了
>
> （2）对于vite构建工具来说，配置文件是：vite.config.js 
>
> ​	vite.config.js 类似于之前webpack当中的vue.config.js，也就是说如果配置代理的话，需要在vite.config.js文件中完成配置。
>
> （3）viet.config.js 配置文件官网：：https://cn.vitejs.dev/config/ 
>
> （4）端口也发生了变化：http://localhost:5173/

#### 9.3 Proxy 实现原理

##### 9.3.1 回顾 object.defineProperty

get 做数据代理。set 做数据劫持。在 set 方法中修改数据，并且渲染页面，完成响应式。

~~~javascript
    <script>
        //创建一个user1对象
        const user1 = {
            name:'jack'
        };
        //创建一个user2对象
        const user2 = {

        }
        //给user2扩展一个属性name
        Object.defineProperty(user2,'name',{
            //数据代理
            get(){
                return user1.name
            },
            //数据劫持
            set(value){
                user1.name=value
            }
        })
    </script>
~~~

存在的问题：

​	1.这种方式只能对修改（set）和读取（get）进行拦截。所以当给对象新增一个属性，或者删除对象的某个属性时，不会经过set和get ，导致无法实现响应式。并且通过数组下标去修改数组中的数据，也不会实现响应式处理。

##### 9.3.2 使用Proxy

------

ES6新特性：window.Proxy对象。

通过Proxy可以创建一个代理对象。

代理模式的原理：通过代理对象去完成目标对象的任务，同时还可以额外新增一些程序。

语法规则：

~~~javascript
        new Proxy(user1, {
            //当修改某个属性的时候，或者新增某个属性的时候，set执行
            set(target, propertyName,value) {
                target[propertyName]=value
            },
            //当读取某个属性的时候，执行get
            get(target, propertyName) {
                //target 参数是代表目标对象
                //propertyName参数代表的是目标对象上的属性名，是一个字符串
                return target[propertyName]
            },
            //当删除某个属性的时候，deleteProperty执行
            deleteProperty(target, propertyName) {
                return delete target[propertyName]
            }
        })
~~~

get方法有两个参数：

> target 参数：代表目标对象
>
> propertyName：代表的是目标对象上的属性名，是一个字符串

set方法有三个参数：

> target 参数：代表目标对象
>
> propertyName 参数：代表的是需要给目标对象添加或修改的属性的属性名，是一个字符串
>
> value 参数:需要给对象添加或修改属性的值

deleteProperty 方法有两个参数：

> target 参数：代表目标对象
>
> propertyName 参数：代表的是需要删除目标对象的属性的属性名，是一个字符串

ES6当中的反射机制：Reflect对象。

#### 9.4 setup

1. setup 是一个函数，vue3 中新增的配置项。

2. 组件中所用到的 data、methods、computed、watch、生命周期钩子....等，都要配置到 setup 中。

3. setup 函数的返回值有两种情况：

> ​	(1) 返回一个对象，该对象的属性、方法等均可以在模板语法中使用，例如插值语法。
>
> ​	(2) 返回一个渲染函数，从而执行渲染函数，渲染页面。
>
> ​			 import {h} from ‘vue’ （引入渲染函数）
>
> ​			return ()=>{h(‘标签体’, ‘标签体的内容’)}
>
> ​		作用是：干掉template中的所有元素，将return中的内容渲染到页面

4. vue3 中可以编写 vue2 语法，向下兼容的。但是不建议。更不建议 vue3+vue2 混用。

~~~javascript
<script>
export default {
  name: 'App',
  setup() {

    //在setuo函数体中定义的变量，可以看作是之前的data
    const name = 'lisi';
    const age = 18;
    //如果想在模板语法中使用name和age
    //就必须将name和age封装成一个对象，然后作为setup函数的返回值返回即可

    //定义一个函数
    function getname() {
      console.log(this)
    };

    return { name, age, getname }
  }
}
</script>
~~~

5.在setup中的 this 是 undefined 。所以想要使用组件中的属性，不能用this.Xxx

~~~javascript
<script>
export default {
  name: 'App',
  setup() {
    console.log(this) //undefined
    function getname() {
      alert(`'姓名：${name},年龄：${age}'`) //可以这样访问
    };
    return { name, age, getname }
  }
}
</script>
~~~

6. 定义的普通属性，是不会做响应式处理的

~~~javascript
export default {
  name: 'App',
  setup() {
      //这样定义的属性是不会做响应式处理的
    const name = 'lisi';
    const age = 18;
    function getname() {
      name = 'zhangsna'
      age=20
    };
    return { name, age, getname }
  }
}
~~~

#### 9.5 ref 函数（实现响应式）

##### 9.5.1 简单数据类型的响应式

1.ref是vue3内置的函数，通过这个函数能够做到响应式。

2.使用ref函数之前需要导入：improt { ref } from ' vue '

3.凡是要实现响应式的data，需要使用ref函数包裹

​	（1）语法：const name = ref(' zhangsan ')

​	（2）输出name，你会发现，它是**RefImpl**对象（引用实现的实例对象，简称引用对象），在这个引用对象当中有一个value属性，			  value属性的实现原理是：Object.defineProperty，通过它实现了响应式处理。

4.读取或修改数据

​		（1）在函数中调用属性的值：属性名.value

​		  (2)   在模板语法中读取属性的值：<p>{{属性名}}</p>

​		（3）修改：属性名.value='新值'

------

##### 9.5.2 对象数据的响应式

1.以下定义的对象是没有响应式处理的

![image-20230814231551381](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230814231551381.png)

2.让对象具有响应式处理

~~~javascript
<script>
import { ref } from 'vue';
export default {
  name: 'App',
  setup() {
    //data
    const userRefImpl = ref({
      name: '张三'
    })

    console.log(userRefImpl) //输出为Proxy对象

    //methods
    function modifyUser() {
      userRefImpl.value.name = '李四'
    }
    return { userRefImpl, modifyUser }
  }
}
</script>
~~~

当对象被ref函数包裹之后，生成的RefImpl对象的value属性不再是普通的对象，而是Proxy对象。

注意：

> 1.对于简单的数据类型来说，通过ref函数做响应式，是通过 object.defineProperty 实现的
>
> 2.对于对象式数据类型来说，通过ref函数做响应式，是通过 object.defineProperty + Proxy 实现的
>
> 3.Proxy实现的响应式，如果是多级嵌套的对象也会具有响应式，也就是对象中的对象中的对象也具有响应式
>
> 4.ref函数跟适合个基本数据类型做响应式处理

#### 9.6 reactive 函数实现对象响应式

reactive 这个函数包裹起来的对象，直接就是一个Proxy。reactive函数不适合与基本数据类型，<u>不能用于基本数据类型</u>。是专门用于对象类型的。Proxy自动递归，即多级嵌套的对象都有响应式。

1.导入

~~~javascript
improt {reactive} from 'vue'
~~~

2.使用

~~~javascript
export default {
  name: 'App',
  setup() {
    const userProxy = reactive({
      name: '张三',
      age: 18
    })
    function modifyUser() {
      userProxy.name = 'lis'
      userProxy.age   = 20
    }
    return { userProxy }
  }
}
~~~

#### 9.7 Vue3中的props

 父组件向子组件传递数据

![image-20230815094331148](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230815094331148.png)

子组件接受父组件传递的数据

![image-20230815094625178](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230815094625178.png)

注意：

1.setup中没有this这个关键字，将来vue3调用setup函数之前，会给setup函数传递参数，第一个参数就是·：props

2.在setup函数中 props 被包装成了一个代理对象，也就是具有响应式的Proxy对象

3.在setup中，不需要 return props对象



#### 9.8 Vue3 的生命周期

![image-20230815095301870](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230815095301870.png)

   							![image-20230815095314259](C:\Users\fengshun\AppData\Roaming\Typora\typora-user-images\image-20230815095314259.png )

注意：

> （1）直接在大括号中进行的配置叫做选项式API（options api）。
>
> （2）在setup函数体中进行的配置，叫做组合式API（ref,reactive这些都属于组合式api）

变化：

> 1.在vue2中 最后两个钩子函数是：beforeDestory（销毁前）和 destroyed（销毁后），在vue3中，变成了beforeUnmount(卸载前)和unmounted(卸载后)
>
> 2.在beforeCreate之前就会执行setup函数
>
> 3.vue3当中，生命周期钩子函数有两种写法
>
> ​	（1）选项式API
>
> ​		![image-20230815102928575](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230815102928575.png)
>
> （2）组合式API
>
> ​	如果使用组合式API，那么setup函数会自动代替beforeCreate和created这两个钩子函数。
>
> vue2 + vue3 组合使用
>
> ~~~javascript
> <script>
>     //引入
> import { onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount, onUnmounted } from 'vue';
> export default {
>     //初始化前
>     beforeCreate() { },
>     //初始化后
>     created() { },
>     setup() {
>         //挂载前
>         onBeforeMount(() => {
> 
>         })
>         //挂载后
>         onMounted(() => {
> 
>         })
>         //更新前
>         onBeforeUpdate(() => {
> 
>         })
>         //更新后
>         onUpdated(() => {
> 
>         })
>         //卸载前
>         onBeforeUnmount(() => {
> 
>         })
>         //卸载后
>         onUnmounted(() => {
> 
>         })
>     },
> }
> </script>
> ~~~

#### 9.9 Vue3中的自定义事件

setup函数中自带有第二个参数：context。

​	绑定事件：

~~~javascript
<template>
  <!-- //给user绑定自定义事件 -->
  <user @event1="showInfo"></user>
</template>

<script>
import user from './components/user.vue'
export default {
  name: 'App',
  components: { user },
  setup() {
    function showInfo(name, age) {
      alert(`姓名:${name},年龄：${age}`)
    }
    return { showInfo }
  }
}
</script>
~~~

触发事件

~~~javascript
<template>
    <button @click="triggerEvent1">触发事件</button>
</template>

<script>
export default {
    setup(props, context) {
        function triggerEvent1() {
            context.emit('event1', '张三', 18)
        }
        return { triggerEvent1 }
    },
}
</script>
~~~

#### 9.10 Vue3中的全局事件总线

1.安装mitt：npm mitt

2.封装event-bus.js文件

> （1）在src目录下新建一个名为 utils 的文件夹，在其中新建一个 event-bus.js 的文件
>
> ![image-20230815114705441](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230815114705441.png)
>
> （2）编写代码
>
> ​		导入mitt库，调用mitt函数并暴露出去，mitt函数会返回一个emitter对象，相当于是全局事件总线对象。
>
> ![image-20230815114917627](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230815114917627.png)
>
> 

3.绑定事件

~~~javascript
<template>
    <brother2></brother2>
</template>

<script>
//导入兄弟组件
import brother2 from './brother2.vue';
//导入全组事件总线
import emitter from '../utils/event-bus'
//引入组合式APi，生命周期钩子
import { onMounted } from 'vue';
export default {
    name: 'brother1',
    components: { brother2 },
    setup() {
        //定义事件触发的函数
        function showInfo(userObj) {
            alert(`姓名：${userObj.name},年龄：${userObj.age}`)
        }
        //生命周期钩子函数：挂载完
        onMounted(() => {
            //给全局事件总线绑定事件
            emitter.on('event1', showInfo)
        })
        return { showInfo }
    }
}
</script>

~~~

4.触发事件

~~~javascript
<template>
    <!-- 添加事件源 -->
    <button @click="triggerEvent1">触发event1事件</button>
</template>

<script>
//导入全组事件总线
import emitter from '../utils/event-bus'
export default {
    name: 'brother2',
    setup() {
        //定义函数
        function triggerEvent1() {
            //触发全局事件总线上的事件event1，并传递参数
            emitter.emit('event1', { name: '张三', age: 32 })
        }
        return { triggerEvent1 }
    }
}
</script>

~~~

5. 移除所有事件和指定事件

~~~javascript
 		function clearAllEvent() {
            //解绑全局事件总线上的所有自定义事假
            emitter.all.clear()
        }
        function clearEvent1() {
            //解绑全局事件总线上的某个事件
            emitter.off('event1')
        }

~~~

#### 9.11 Vue3的计算属性

在vue3中，computed是一个组合式API

1.导入

~~~javascript
import { computed} from 'vue'
~~~

2.简单写法

~~~javascript
let reversedContext = computed(() => {
      return data.context.split('').reverse().join('')
    })
~~~

3.完整写法

~~~javascript
let letctoLowerContext = computed({
      get() {
        //当读取计算属性的值的时候执行
        return data.context.toUpperCase()
      },
      set(value) {
        //当修改计算属性值的时候执行
        data.context = value.toLowerCase()
      }
    })
~~~

#### 9.12 Vue3中的侦听属性

<u>被监视的数据必须是具有响应式的</u>。

1.导入

~~~javascript
import watch from 'vue'
~~~

2.侦听ref包裹的简单数据

> （1）普通写法
>
> ~~~javascript
>     watch(data, (newValue, oldValue) => {
>         
>       console.log(newValue,oldValue)
>         
>     },{immediate:true,deep:true})
> ~~~
>
> （2）数组写法
>
> 监视多个属性，当监视的多个属性的逻辑相同时，可以使用数组写法
>
> ~~~javascript
>  watch([context1,context1], (newValue, oldValue) => {
>         //newValue 和 oldValue 都是一个数组
>       console.log(newValue,oldValue) //两个属性发生变化时，都会触发一样的代码
>         
>     },{immediate:true,deep:true})
> ~~~

3.侦听reactive包裹的对象形式

> （1）侦听整个对象
>
> ~~~javascript
>     watch(data, (newValue, oldValue) => {
>         
>       console.log(newValue,oldValue)
>         
>     })
> ~~~
>
> （2）侦听对象中的某个属性
>
> 第一个侦听的属性必须是函数
>
> ~~~javascript
>     watch(()=>{return data.context}, (newValue, oldValue) => {
>         
>       console.log(newValue,oldValue)
>         
>     })
> //简写形式
> watch(()=>data.context, (newValue, oldValue) => {
>         
>       console.log(newValue,oldValue)
>         
>     })
> ~~~
>
> （3）侦听多个属性
>
> ~~~javascript
> watch([()=>data.content1,()=>data.content2], (newValue, oldValue) => {
>         //newValue 和 oldValue 都是一个数组
>       console.log(newValue,oldValue)
>         
>     })
> ~~~
>
> （4）侦听对象中的对象
>
> ~~~javascript
> watch(data.adress, (newValue, oldValue) => {
>         
>       console.log(newValue,oldValue)
>         
>     })
> ~~~
>
> 注意：
>
> 1.侦听reactive包裹的对象，vue目前存在缺陷，只会获取到newValue的值，不会获取到oldValue的值
>
> 2.如果监视的 reactive 定义的响应式数据。强制开启了深度监视，配置 deep 无效。默认就是监视对象的全部属性

#### 9.13 watchEffect 函数

watchEffect也是个组合式API，用来监视数据变化的。

watchEffect 函数里面直接写一个回调函数，回调函数中用到哪个属性，当这个属性发生变化的时候，这个回调就会重新执行。

1.导入

~~~javascript
import {watchEffect} from 'vue'
~~~

2.使用

~~~javascript
    watchEffect(() => {
        //只要context1或者context2发生变化，这个回调函数就会执行
      const a = data.context1
      const b = data.context2
      //a和b的值就相当于是newValue
      console.log(a,b)
    })
~~~

#### 9.14 自定义hook函数（自定义钩子函数）

hook函数类似于vue2中的mixins混入。

1.在src中新建一个名为hooks的文件，在hooks文件夹在新建一个js文件

![image-20230815174140012](C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20230815174140012.png)

2.在sum.js中编写钩子函数

~~~javascript
import { reactive } from 'vue'
export default function () {
    //data
    const data = reactive({
        num1: 0,
        num2: 2,
        result: 0
    })
    //methods
    function sum() {
        data.result = data.num1 + data.num2
    }
    //返回一个对象
    return { data, sum }
}
~~~

3.在需要使用sum函数的地方导入文件

~~~javascript
import sum from'./hooks/sum.js'
~~~

4.使用钩子函数

~~~html
<template>
    <button @click="sum"></button>
</template>

<script>

export default {
    import sum from './hooks/sum.js'
    name: 'brother1',
    components: { brother2 },
    setup() {
   /*
    //调用钩子函数 
    const sum = sum()
    //返回一个对象
    return sum*/
    
    //直接返回
    return sum()
    }
}
</script>

~~~

#### 9.15 shallowReactive和shallowRef

浅层次的响应式。

shallowReactive：对象的第一层支持响应式，第二层就不再支持了。

shallowRef：只给基本数据类型添加响应式。如果是对象，则不会支持响应式。

~~~javascript

import { shallowRef, shallowReactive } from 'vue'
export default {
    name: 'shallowTest',
    setup() {
        //针对基本数据，使用shallowRef，对象具有响应式
        let num = shallowRef(20)
        //针对对象，使用shallowRef，对象不再具有响应式
        const data = shallowRef({
            name: 'zhangsna'
        })
        //针对对象，使用shallowReactive，对象的第一层支持响应式
        const user = shallowReactive({
            address: "地址",
            person: {
                pthone: 15303,
                name: 'zhangsna'
            }

        })
    }
}
~~~

为什么要使用shallowRef？

> 被shallowRef的对象不具有响应式，后期不会对对象中的某个属性进行修改，即使需要修改，也是替换整个对象。此外 对象.value具有响应式

#### 9.16 组合式API和选项式APi对比

组合式API：compoentsition API

选项式API：Options API

对于选项式API来说，关注点子在选项上，data配置项中存放了所有功能的data数据，methods配置项中存放了所有功能所需的方法等。在维护某一个功能时，需要在data配置项中修改数据，在methods中找到对应的方法进行修改，导致维护起来比较困难。

对于组合式API，关注点在功能上，组合式API配合 hook钩子，做到一个文件对应一个功能，文件中包含这个功能所需的data，methods等，这样维护起来更容易。

#### 9.17 深只读与浅只读

深只读：具有响应式的对象中的所有的属性，包括子对象中的所有子对象，它的所有值都是只读的，不可修改

浅只读：具有响应式的对象中的第一层属性值是只读的

语法：

> 深只读：readonly(对象)
>
> 浅只读：shallowReadonly(对象)

~~~javascript

import { readonly,shallowReadonly,reactive } from 'vue'
export default {
    name: 'shallowTest',
    setup() {
        //定义一个响应式对象
        const data=reactive({
            name:'zhansgan',
            person:{
                pheno:126
            }
        })
        //深只读
        data=readonly(data)
        //浅只读
        data=shallowReadonly(data)
        })
    }
}
~~~

作用：其他组价给当前组件传递数据，但是希望当前组件不能修改该数据



#### 9.18 响应式数据的判断

------

isRef:检查某个值是否为ref。

isReactive:检查一个对象是否有 reactive() 或 shallowReactive() 创建的代理。

isProxy:检查一个对象是否是由 reactive() 、readonly()、 shallowReactive() 或 shallowReadonly 创建的代理。

isReadonly：检查传入的值是否为只读对象。

#### 9.19 toRef与toRefs

在模板语法中，重复使用数据比较麻烦，例如：{{data.a.b.c.d}}

使用toRef进行解决：

> 语法：toRef(对象名,'对象的属性')
>
> 例：
>
> ~~~javascript
> import {toRef } from 'vue'
> export default {
>     name: 'shallowTest',
>     setup() {
>         //定义响应式数据
>         const data=reactive({
>             content:1,
>             person:{
>                 name:'张三',
>                 phone:134,
>                 address:{
>                     city:'城市'
>                 }
>             }
>         })
>         //返回
>         return{
>             content:toRef(data,'content'),
>             phone:toRef(data.person,'phone'),
>             city:toRef(data.person.address,'city')
>         }
>     }
> }
> ~~~
>
> 可以在模板语句中直接使用了
>
> ~~~html
> <p>{{city}}</p>
> ~~~
>
> 注意：toRef函数执行之后生成了一个全新的对象：objectRefImpl（引用对象）,只要有引用对象，就有value属性，并且value属性是响应式的（value对应有set和get方法）

使用toRefs：

> toRefs函数生成的是一个对象，他能代替我们生成以上的代码。
>
> ~~~javascript
>     import {toRefs } from 'vue'
>     export default {
>         name: 'shallowTest',
>         setup() {
>             //定义响应式数据
>             const data=reactive({
>                 content:1,
>                 person:{
>                     name:'张三',
>                     phone:134,
>                     address:{
>                         city:'城市'
>                     }
>                 }
>             })
>             //返回
>             return{
>                ...toRefs(data)
>             }
>         }
>     }
> ~~~
>
> 注意：toRefs只能帮助我们省略data字段
>
> 例如：
>
> ~~~html
> <p>{{person.name}}</p>
> ~~~

#### 7.20 转换为原始 & 标记为原始

toRaw：将响应式对象转换为普通对象。只适合与reactive生成的响应式对象。对原始对象中的数据进行操作，原始数据会变，但是不会做响应式处理

markRaw:标记某个对象，让这个对象永远都不具备响应式。比如在集成一些第三方库的时候，比如有一个巨大的只读列表，不让其具备响应式是一种性能优化。

toRaw的运用场景：后期处理业务的时候，处理到某个环节的时候，希望修改data的时候取消响应式

~~~javascript
import {toRaw，markRaw，reactive } from 'vue'
export default {
    name: 'shallowTest',
    setup() {
        //定义响应式数据
        const data=reactive({
            content:1,
        })
        //获取data的原始对象
        function getRaw(){
            let RawData = toRaw(data)
        }
        function addMarkRaw(){
            //为data添加一个非响应式的对象
            data.x=markRaw({num:1})
        }
        //返回
        return{
           data
        }
    }
}
~~~

#### 7.21 Fragment 组件

Fragment翻译为：碎片、片段。

在Vue2中每个组件必须有一个根组件1.这样性能方面稍微有点问题，如果每个组件必须有根标签，组件嵌套组件的时候，有很多无用的根标签。

在Vue3中每个组件不需要有根标签。实际上内部实现的时候，最终将所有的组件嵌套好之后，最外层会添加一个Fragment，用这个 Fragment 当做根标签。这是一种性能优化策略

#### 7.22 Teleport 组件

Teleport 翻译为：远距离传送。用于设置组件的显示位置。

语法：<teleprot to='选择器'> 标签</teleprot >

运用：多级嵌套下的div，例如：div>div>div>div>弹窗 ，当某个父级元素添加了绝对定位，若某个父级元素的尺寸发生了变化，弹簧也会被影响。此时可以想办法把弹窗移动到别处，不再嵌套到div下，例如移动到body下。就可以使用 teleprot 

~~~html
<template>
	<div>
        <div>
            <div>
  				<Teleport to="body">
    				<div class="cover" v-show="data.isshow">
      					<div class="s">
        					<h1>这是一个弹窗....</h1>
        					<button @click="data.isshow = false">关闭弹窗</button>
      					</div>
    				</div>
  				</Teleport>
    	 </div>
       </div>
    </div>
</template>
<style>
.cover {
  position: absolute;
  /* 扩充到整个页面 */
  left: 0;
  top: 0;
  bottom: 0;
  right: 0;
  /*设置透明度 */
  opacity: 50%;

  background-color: #090808;
}

.s {
  position: absolute;
  left: 40%;
  top: 40%;
  width: 300px;
  height: 300;
  background-color: aqua;
}
</style>
~~~

#### 7.23 隔代数据传递

​	在vue2中可以使用props、自定义事件、全局时间总线进行传递数据。在Vue3中，提供了一个新的数据传递，用于多级嵌套的数据传输。

1.在祖宗组件中使用 provide 提供数据。

> 导入
>
> ~~~javascript
> import {provide} form 'vue'
> ~~~
>
> 传递数据
>
> ~~~javascript
> //privide('数据名'，数据)
> let countent = ref(1)
> privide('c',content)

在后代组件中使用 inject 完成数据的注入。

> 导入
>
> ~~~java
> import {inject} form 'vue'
> ~~~
>
> 接收数据
>
> ~~~javascript
> let content=inject('c')

这种方式比较适合于使用在祖宗给后代组件传递数据的情况

#### 9.24 自定义ref

##### watch 的方式实现延迟显示

> 需求：在文本框中输入字符，下面的p标签延迟一秒显示输入的字符
>
> ~~~html
> <template>
>     <input type="text" v-model="name">
>     <br>
>     <p>{{ newName }}</p>
> </template>
> 
> <script>
> import { ref, watch } from 'vue'
> export default {
>     name: 's',
>     setup() {
>         const name = ref('');
>         let newName = ref(name.value);
>         watch(name, (newValue, oldValue) => {
>             setTimeout(() => {
>                 newName.value = newValue
>             }, 1000)
> 
>         })
>         return { name, newName }
>     }
> 
> }
> </script>
> 
> <style></style>
> ~~~

##### 自定义ref

使用自定义的ref 同样具有响应式

~~~html
<template>
    <input type="text" v-model="name">
    <p>{{ name }}</p>
</template>

<script>
import { customRef } from 'vue'
export default {
    name: 'suer',
    setup() {
        //创建一个防抖 ref
        //以下代码才是自定义的ref
        //ref是一个函数
        function useDebouncedRef(value) {
            let t
            //自定义的ref这个函数体当中的代码不能随便写，必须符合ref规范
            /*
                1.调用customRef 函数时必须给该函数传递一个回调：这个回调可以叫做factory
                2.在这个回调函数中有两个非常重要的参数：track, trigger

            */
            const x = customRef((track, trigger) => {

                //对于这个factory回调函数来说，必须返回一个对象，并且对象要有get
                return {
                    //模板语句中只要使用到该属性就自动调用
                    get() {
                        //告诉vue追踪value的变化
                        track()
                        return value
                    },
                    //模板语句中主要修改该属性就自动调用
                    set(newvalue) {
                        clearTimeout(t)
                        t = setTimeout(() => {
                            value = newvalue
                            //通知调用get方法
                            trigger()
                        }, 1000)

                    }
                }
            })

            //返回自定义ref对象
            return x
        }
        let name = useDebouncedRef('test')
        return { name }
    }
}
</script>

<style></style>
~~~

#### 9.25 vue3语法糖setup

在之前的语法中，需要在组件中使用setup函数，并且在函数中定义各种函数、数据、钩子函数等，最后还需要使用return返回出去，template中才能使用。

为了简化操作，可以在script标签中加入 setup 属性，它会自动将我们定义好的属性、函数等return出去，并且将组件暴露出去，也就是说不再需要使用export default

~~~html
<template></template>

<script setup>
import { ref } from 'vue';
import user from './components/user.vue'
name: 'test'
let username = ref('zhangsna')
function sum() {
    alert('1111111111')
};
//watch
//computed
//....
</script>

<style></style>
~~~

好处：

1.不在需要使用setup函数，也不需要使用return

2.不再需要将组件暴露出去

3.引入其他组件时，不需要注册，直接引入后使用即可

##### 9.25.1 在组合式API中使用父传子

父组件向子组件传递数据：

![image-20231009201850383](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231009201850383.png)

子组件接收数据：

![image-20231009201908742](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231009201908742.png)

传递响应式数据：

![image-20231009202538652](C:\Users\fengshun\Desktop\自学\微服务框架\SpringBoot\笔记\images\image-20231009202538652.png)

##### 9.25.2 在组合式API中使用子传父

1. 父组件中给子组件标题通过@绑定事件

   ~~~vue
   <template>
       <!--1. 绑定定义事件 -->
       <son @event1="getmsg"></son>
   </template>
   
   <script setup>
   // 引入子组件
   import son from "./components/son.vue"
   const getmsg = (msg) => {
       console.log(msg);
   }
   </script>
   
   <style></style>
   ~~~

   

2. 子组件内部通过 $emit 方法触发事件

   ~~~vue
   <template>
       <button @click="sendmsg">向父组件传递数据</button>
   </template>
   
   <script setup>
   // 2. 通过 defineEmits 编译器宏生成emit方法
   const emit = defineEmits(["event1"])
   const sendmsg = () => {
       // 触发自定义事件，并传递参数
       emit("event1", "hjkjbak")
   }
   </script>
   
   <style></style>
   ~~~

##### 模板应用

通过 ref 标识获取真实dom对象或组件实例对象

使用：

> 1. 绑定
>
> ~~~vue
> <script setup>
> // 引入子组件
> import { ref } from "vue";
> // 1.调用ref函数得到red对象
> const h1ref = ref(null)
> </script>
> 
> <template>
>     <!-- 2.通过ref标识绑定ref对象到标签 -->
>     <h1 ref="h1ref">dom标签</h1>
> </template>
> 
> <style></style>
> ~~~
>
> 2.使用：
>
> 注意：只有组件挂载完毕之后才能获取
>
> <script setup>
> // 1.调用ref函数得到red对象
> const h1ref = ref(null)
> onMounted(() => {
>     console.log(h1ref.value)
> })
> </script>

如果 ref 标记的是子组件，那么控制台输出的是 Proxy 对象，其中子组件的属性并不一定公开。

原因：默认情况下在 <script setup> 语法糖中组件内部的属性和方法是不开放给父组件访问的，可以通过defineExpose 编译宏指定哪些属性和方法允许访问。

1. 子组件中暴露出公开的属性或者方法：

   ~~~vue
   
   <script setup>
   
   import { ref } from 'vue'
   
   const message = ref(6651)
   defineExpose({
       message
   })
   </script>
   
   <template>
       <h1>sdicugisdvc</h1>
   </template>
   
   ~~~

2. 父组件中接收：

   ~~~vue
   
   
   <script setup>
   import son from './components/son.vue';
   import { onMounted, ref } from 'vue';
   const sonref = ref(null)
   onMounted(() => {
       console.log(sonref.value);
   })
   </script>
   
   <template>
       <son ref="sonref"></son>
   </template>
   ~~~

##### provide 和 inject 

   作用：顶层组件向任意的底层组件传递数据和方法，实现跨层组件通信

<img src="C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20231011095030674.png" alt="image-20231011095030674" style="zoom:67%;" />

跨层传递普通数据

> 1. 顶层组件通过provide函数提供数据
>
>    ~~~vue
>    <script setup>
>    // 顶层组件传递数据
>    provide('message', 'this is toom data')
>    </script>
>    ~~~
>
> 2. 底层组件通过inject函数获取数据
>
>    ~~~vue
>    <template>
>        <h2>底层组件</h2>
>        <h2>{{ message }}</h2>
>    </template>
>                
>    <script setup>
>    import { inject } from "vue";
>                
>    const message = inject('message')
>    </script>
>                
>    ~~~
>
>    注意：如果顶层组件传递的数据具有响应式，也可以通过底层组件来修改

跨层传递方法

> 顶层组件可以向底层组件传递方法，底层组件调用方法修改顶层组件中的数据
>
> 1. 顶层组件中定义方法，并通过provide函数传递
>
>    ~~~vue
>    
>    
>    <script setup>
>    import son from './components/son.vue';
>    import { onMounted, provide, ref } from 'vue';
>    
>    const setNumber = () => {
>        number.value++
>    }
>    
>    // 顶层组件传递方法
>    provide('number-key', setNumber)
>    </script>
>    
>    <template>
>        <p>{{ number }}</p>
>        <son ref="sonref"></son>
>    </template>
>    ~~~
>
>    
>
> 2. 底层组件通过inject函数接收
>
>    ```vue
>    <template>
>        <h2>底层组件</h2>
>        <button @click="setNumber">修改顶层组件中的number</button>
>    </template>
>    
>    <script setup>
>    import { inject } from "vue";
>    
>    const setNumber = inject('number-key')
>    </script>
>    
>    <style></style>
>    ```

#### 9.26 使用vite 解决ajax跨域问题

##### 前提

前端：vue3+axios

后端SpringBoot：

##### 1.前端配置（代理服务器）：

在vite.config,js中如下配置：

~~~javascript
  server: {
    proxy: {
      '/file': {
        target: 'http://localhost:9000/backend', // 你请求的第三方接口
        changeOrigin: true, // 在本地会创建一个虚拟服务端，然后发送请求的数据，并同时接收请求的数据，这样服务端和服务端进行数据的交互就不会有跨域问题
        rewrite: path => path.replace(/^\/file/, '') // 重写路径，将 /api 变为 /
      }
    }
  }
~~~

发送ajax请求：

~~~javascript
const select = () => {
  axios.get("/file/test", {
    params: data
  }).then(
    responce => { console.log('responce.data :>> ', responce.data); },
    error => { console.log('error :>> ', error.message); }
  )
}
~~~

##### 2.后端配置

> 1. 创建 request.js，创建axios实例
>
>    在项目根目录下，也就是src目录下创建文件夹api/，并创建`request.js` ，该js用于创建axios实例。
>
>    ~~~javascript
>    import axios from "axios";
>    const api = axios.create(
>    	{ 
>    		baseURL: "http://localhost:8081", //这里配置的是后端服务提供的接口
>    		timeout: 1000 
>    	}
>    );
>    export default api;
>    
>    ~~~
>
> 2. 在main.js中全局注册axios
>
> 3. 在页面中使用axios
>
>    ~~~vue
>     <el-button class="login_button" type="primary" @click="login"
>          >登录</el-button>
>    
>    <script setup>
>    import { reactive } from "vue";
>    import api from "@/api/request.js"; //引入api
>    //测试请求方法
>    const login = function () {
>      api({ url: "/test", method: "get" }).then((res) => {
>      alert("请求成功!");
>      console.log(res);
>       		}
>       );
>    
>    ~~~
>
> 4. 在后端配置跨域问题
>
>    1. 解决单Contoller跨域访问
>
>       ~~~java
>       	@CrossOrigin(origins ="*" ,maxAge = 3600)
>           @GetMapping("/test")
>           public ApiResult test() {
>               return ApiResultHandler.buildApiResult(200, "hello!", null);
>           }
>       
>       ~~~
>
>    2. 全局解决跨域问题
>
>       自己创建一个config包，创建CorsConfig类。
>
>       ~~~java
>       import org.springframework.context.annotation.Bean;
>       import org.springframework.context.annotation.Configuration;
>       import org.springframework.web.cors.CorsConfiguration;
>       import org.springframework.web.cors.UrlBasedCorsConfigurationSource;
>       import org.springframework.web.filter.CorsFilter;
>       
>       @Configuration
>       public class CorsConfig {
>           /**
>            * 当前跨域请求最大有效时长。这里默认1天
>            */
>           private static final long MAX_AGE = 24 * 60 * 60;
>       
>           @Bean
>           public CorsFilter corsFilter() {
>               UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
>               CorsConfiguration corsConfiguration = new CorsConfiguration();
>               // 1 设置访问源地址
>               corsConfiguration.addAllowedOrigin("*");
>               // 2 设置访问源请求头
>               corsConfiguration.addAllowedHeader("*");
>               // 3 设置访问源请求方法
>               corsConfiguration.addAllowedMethod("*");
>               corsConfiguration.setMaxAge(MAX_AGE);
>               // 4 对接口配置跨域设置
>               source.registerCorsConfiguration("/**", corsConfiguration);
>               return new CorsFilter(source);
>           }
>       }
>       
>       ~~~
>
>       
>
> ​	



### Pinia

Pinia是vue的专属的最新状态管理库，是vuex状态管理工具的替代品

特点：

1. 提供更加简单的API（去掉了 mutation）
2. 提供符合组合式风格的API（和vue3新语法统一）
3. 去掉了modules 的概念，每一个store 都是一个独立的模块
4. 搭配了TypeScript一起使用提供可靠的类型推断

------

#### 使用

1. 安装：

   > ~~~
   > npm install pinia
   > ~~~

2. 创建一个 pinia 实例 (根 store) 并将其传递给应用

   > ~~~
   > import { createApp } from 'vue'
   > import { createPinia } from 'pinia'
   > import App from './App.vue'
   > 
   > const pinia = createPinia()
   > const app = createApp(App)
   > 
   > app.use(pinia)
   > app.mount('#app')
   > ~~~
   >
   > 

#### 第一个案例：

1. 先创建一个Store

   在src目录下新建 Stores/counter.js 文件

   使用对象式：

   ~~~javascript
   // stores/counter.js
    
   
   export const useCounterStore = defineStore('counter', {
     state: () => {
       return { count: 0 }
     },
     // 也可以这样定义
     // state: () => ({ count: 0 })
     actions: {
       increment() {
         this.count++
       },
     },
   })
   ~~~

   使用函数形式：（推荐使用函数形式）

   ~~~javascript
   import { defineStore } from "pinia";
   import { ref } from "vue";
   export const useConunterStore = defineStore('counter', () => {
       // 定义数据（State）
       const count = ref(0)
   
       // 定义修改数据的方法（action 支持同步和异步）
       const increment = () => {
           count.value++
       }
   
       // 以对象的方式return 供组件使用
       return {
           count,
           increment
       }
   })
   //注意：
   1.使用函数形式，最后需要return 出去
   2.defineStore是一个方法，定义一个参数来接收该函数并且最后需要暴露出去，其返回值就是一个代理对象Proxy
   3.用来接收的参数命名规则：user+‘defineStore中的自定义key’+Store
   4.在需要使用Store的组件中引入该文件并且调用该方法即可
   ~~~

2. 在组件中使用：

   ~~~vue
   <script setup>
   import son from './components/son.vue';
   import { onMounted, provide, ref } from 'vue';
   // 1.导入 use 开头的方法
   import { useConunterStore } from './stores/counter'
   // 2.执行方法得到store实例对象
   const ConunterStore = useConunterStore()
   console.log('ConunterStore :>> ', ConunterStore);
   
   </script>
   
   <template>
   
       <button @click="ConunterStore.increment">{{ ConunterStore.count }}</button>
   </template>
   ~~~

#### getter 和 异步action

在pinia中 getter 相当于vuex中计算属性，

pinia中的getter直接使用computed函数进行模拟

**使用getter**：

> ~~~javascript
> import { defineStore } from "pinia";
> import { computed, ref } from "vue";
> export const useConunterStore = defineStore('counter', () => {
>     // 定义数据（State）
>     const count = ref(0)
> 
>     // 定义修改数据的方法（action 支持同步和异步）
>     const increment = () => {
>         count.value++
>     }
> 
>     //getter
>     const doubleCount = computed(() => {
>         return count.value * 2
>     })
> 
>     // 以对象的方式return 供组件使用
>     return {
>         count,
>         doubleCount,
>         increment
> 
>     }
> })
> ~~~
>
> 使用：
>
> ~~~vue
> <script setup>
> import son from './components/son.vue';
> import { onMounted, provide, ref } from 'vue';
> // 1.导入 use 开头的方法
> import { useConunterStore } from './stores/counter'
> // 2.执行方法得到store实例对象
> const CounterStore = useConunterStore()
> 
> console.log('CounterStore :>> ', CounterStore);
> console.log('CounterStore.doubleCount :>> ', CounterStore.doubleCount);
> 
> </script>
> 
> <template>
> 
>     <button @click="CounterStore.increment">{{ CounterStore.count }}</button>
>     <p>{{ CounterStore.doubleCount }}</p>
> </template>
> ~~~

**action 如何实现异步**：

action中实现异步和组件中定义数据和方法的风格完全一致：

~~~java
import { defineStore } from "pinia";
import { computed, ref } from "vue";
import axios from "axios";
export const useConunterStore = defineStore('counter', () => {
    // 定义数据（State）
    const API_URL = 'http://geel/itheima.net/v1_0/channels'
    // 准备数据
    const list = ref([])
    // 异步action
    const LoadList = async () => {
        const res = await axios.get(API_URL)
        list.value = res.data.data.channels
    }
    // 以对象的方式return 供组件使用
    return {
        LoadList

    }
})
~~~

#### storeToRefs

使用 storeToRefs 函数可以辅助保持数据（state+getter） 的响应式解构

一下代码会让响应式丢失：

~~~vue
const {count,doubleCount}=CounterStore
~~~

使用 storeToRefs 可以保持响应式：

1. 导入

   ~~~javascript
   import { storeToRefs } from 'pinia'
   ~~~

2. 解构：

   ~~~javascript
   const { count, doubleCount } = storeToRefs(CounterStore)
   ~~~

3. 使用：

   ~~~html
       <input type="text" name="" id="" v-model.number="count">
   ~~~

对于store中的方法，不能使用 storeToRefs 函数解构，但是普通解构依旧有效

~~~javascript
const { increment } = CounterStore
//其中 increment 为函数
~~~

### scss 文件的自动导入

为什么要自动导入：

在项目中一些组件共享的色值会议scss变量的方式统一放到一个名为 var.scss 的文件中，正常组件中使用，需要先导入scss文件，在使用内部的变量，比较繁琐，自动导入可以免去手动导入的步骤，直接使用内部的变量。

这是一个scss文件：

<img src="C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20231011195332991.png" alt="image-20231011195332991" style="zoom:50%;" />

使用手动导入：

<img src="C:\Users\fengshun\Desktop\自学\Vue\视屏\image-20231011195400081.png" alt="image-20231011195400081" style="zoom:50%;" />

使用自动导入：

   > 1. 新增一个var.scss 文件，存入色值变量
   > 2. 通过 vite.config.js 配置自动导入文件
