# JavaScript

## 节点操作

### 复制节点（克隆）

~~~
node.cloneNode()
~~~

**注意：**

1. 如果参数为空或为false，则为浅拷贝，即只克隆复制节点本身，不克隆里面的子节点

2. true是深拷贝

~~~js
var data =[{
    name:'xiaobao',
    subject:'js',
    score:100
},{
    name:'xiaoguaiguai',
    subject:'js',
    score:100
}]
var tbody = document.querySelector('tbody')
for (var i=0;i<data.length;i++){
    var tr = document.createElement('tr');
    tbody.apendpChild(tr);
    for(var k in data[i]){
        var td = docment.creatElement('td');
        td.innerHTML = data[i][k]
        tr.apendChild(td);
    }
    // 创建删除单元格
    var td = document.creartElement('td')
    td.innerHTML = '<a href="javascript:;">删除</a>'
    tbody.apendpChild(td)
}

// 删除操作开始
var as =docunment.querySeletor('a');
for(var i =0;i<as.length;i++){
    as[i].onclick = function(){
        tbody.removeChild(this.parentNode.parentNode)
    }
}
~~~

### 三种动态创建元素区别

+ `document.write()`
+ `element.innerHTML`
+ `document.creatElement()`

**区别**

1. `document.write()`直接将内容写入的内容流，<u>但是文档流执行完毕，页面会重绘</u>
2. `element.innerHTML`的效率更高，并且不会导致页面重绘，结构稍微复杂，不要用拼接字符串的形式
3. `creatElement()` 结构更清晰，但是效率更低一点

## DOM重点核心

关于`dom`操作，我们主要针对元素的操作，主要有创建、增、删、改、查、属性操作、事件操作

### 创建

1. `document.write`
2. `innerHTML`
3. `createElement`

### 增

1. `apendChild`
2. `insertBefore`

### 删

1. `removeChild()`

### 改

主要更改元素的属性，`dom`元素的内容、属性和表单值

1. 修改元素属性：`src、href、title`等
2. 修改普通元素内容：`innerHTML、innerText`
3. 修改表单元素：`value、type、disable`
4. 修改元素演示：`style、className`

### 查

1. `DOM`提供的`API`方法：`getElementById、getElementByTagName`古老用法不推荐
2. `H5`提供的新方法：`querySelector、querSelectorALL`提倡
3. 利用节点操作获取元素：父`（parentNode）`、子（`children`）、兄（`previousElementSilbling、nextElemnetSilbling`）提倡

### 属性操作

主要针对自定义属性

1. `setAttribute`：设置`dom`的属性值
2. `getAttribute`：得到`dom`的属性值
3. `removeAttribute`：移除属性

### 事件操作

给元素注册事件，采取事件源.事件类别=事件处理程序

## 事件高级

### 注册事件

注册事件有两种方式，传统方式和方法监听注册方式

#### 传统注册方式

+ 利用on开头的事件`onclick`
+ 特点：注册事件的唯一性，同一元素同一事件只能设置一个处理函数，最后注册的处理函数会覆盖之前的

#### 方法监听注册方式

+ `w3c`标准推荐方式
+ `addEventListener()`可以多个注册，按顺序依次执行
+ IE9之前使用attachEvent，不推荐

#### addEventListener事件监听

````javascript
eventTarget.addTventListener(type,listener,useCapture)
````

+ type:事件类型字符串，如click,mouserover 注意不加on
+ listener：事件处理函数
+ useCapture：可选，默认为false，如果为true,则处于捕获阶段，否则为冒泡阶段。

### 删除事件

1. 传统删除方式：`eventTarget.onclick=null`
2. 方法监听注册方式`eventTarget.removeEventListener(type,listener,useCapture)`要写绑定函数的名字，所以不能写匿名函数

### DOM事件流

事件在元素节点之间的顺序传播过程被称为DOM事件流，事件流有三个步骤：

1. 捕获阶段
2. 事件阶段
3. 冒泡阶段

事件处于捕获阶段时，从上往下执行，先执行`document`，再执行`html`，再执行`body`，再执行`father`,最后是`son`，模式影响执行的先后顺序

**注意：**

+ `JS`只能执行捕获或者冒泡中的一个阶段
+ `onclick`和`attachEvent`只能得到冒泡阶段
+ `addTventListener`的第三个参数影响是否冒泡阶段
+ 实际开发中很少使用捕获，更关注冒泡
+ 有些事件没有冒泡如`onblur、onfocus、onmouseenter、onmouseleave`

### 事件对象

事件函数的形式参数，不需要传参，依赖于事件，常用`event、evt等`命名

事件对象中有很多属性，如鼠标事件的坐标等等：

+ `e.target`触发事件的对象（如点击的是`ul`中的`li`），区别于`this`（谁绑定就是谁）
+ `e.type`事件类型
+ `e.preventDefault`阻止默认行为，如，让链接不跳转，按钮不点击
+ `e.returnValue` 是属性，设置它来阻止默认行为，低版本的可以使用
+ `reture false` 不存在兼容性问题，但是只限于传统的注册行为，且之后的代码不执行

### 阻止事件冒泡

事件冒泡：开始时由最具体的元素接受，然后逐级船舶，直至`DOM`最顶点的节点

#### 阻止方法

+ 标准写法：利用事件对象里面的`e.stopPropagation()`方法
+ `e.cancelBubble=true`低版本的ie

### 事件委托（代理、委派）

#### 原理

不是每个子节点单独设置事件监听器，而是事件监听器设置在父节点上，然后利用冒泡原理影响设置每个子节点

案例：给`ul`注册点击事件，而后利用事件对象的`target`来找到当前点击的`li`，因为点击`li`事件会冒泡到`ul`上

#### 作用

操作一次`DOM`即可，提高了性能

### 常见的鼠标事件和键盘事件

+ `e.clientX、e.clientY、(可视区域)e.pageX、e.pageY（页面）、e.screenX、e.screenY（屏幕）`
+ 键盘事件：`onkeyup、onkeydown、onkeypress(不识别功能键)` 有顺序`onkeydown=>onkeypress=>onkeyup`
+ `e.key（存在兼容问题）、e.keyCode`  `onkeypress`才区分大小写

案例：

~~~js
var pic = document.queryselector('img')
document.addEventListener('mousemove',function(e){
    var x = e.page.X
    var y = e.page.Y
    //要加单位，否则不能用
    pic.style.left = x + 'px'
    pic.style.right = y + 'px'
})
~~~

## BOM对象

### BOM概述

浏览器对象模型，浏览器被当作一个对象，BOM的顶级对象是window，BOM用来交互，浏览器定义，兼容性差

+ 是JS访问浏览器的一个接口
+ window是全局对象，全局变量和方法会变成window的成员
+ window对象有一个特殊属性name

### window对象的常见事件

#### 窗口加载事件

`window.onload=function(){}或者window.addEventListener("load",funchtion(){})`等页面加载完成后才执行代码

`DOMcontentloaded`事件，页面所有的文字加载完成的事件，防止图片太多影响交互

#### 调整浏览器窗口大小的事件

`resize`事件，浏览器窗口大小变化，一般用于响应式的操作，例如浏览器窗口大小`window.innerWidth`变化就隐藏一些内容

### 页面加载事件

### 两种定时器及其区别

####  setTimeout()

+ `window.setTimeout(调用函数,[延迟毫秒数])`
+ 一次性的，执行一次
+ window可以省略，调用函数可以直接写变量，也可以是 `函数名()`
+ 可以将定时器保存为变量
+ 异步执行的函数一般被称为回调函数

~~~js
var ad =document.querySelector('.ad')
setTimeout(function(){
    ad.style.display = 'None'
},5000)
~~~

#### 停止setTimeout()

`window.clearTimeout(定时器的名字)`

#### setInterval()

`window.setInterval(调用函数,[延迟毫秒数])`，每隔多少秒就执行一次

案例：秒杀倒计时

~~~js
var hour = document.querySelector('.hour')
var minute = document.querySelector('.minute')
var second = document.querySelector('.second')
var inputTime = + new Date('一个时间');
coutDown()
setTimeout(coutDown,1000)
function coutDown(){
    var nowTime = + new Date();
    var times = (inputTime - nowTime) / 1000 ;
    var h = parseInt(time / 60 / 60 % 24) ;
    h = h < 10 ? '0' + h : h
    hour.innerHTML = h
    var m = parseInt(times / 60 % 60);
    m = m < 10 ? '0' + m : m;
    minute.innerHTML = m
    
    var s = parseInt( time % 60);
    s = s < 10 ? '0' + s : s;
    second.innerHTML = s
}
~~~

#### 停止定时器setInterval()

`window.clearInterval(定时器的名字)`，要注意全局变量保存定时器

~~~js
var btn = document.querySelector('button')
var time = 10
btn.addEventListener('click',function(){
    btn.disable = true;
    var timer = setInterval(funtion(){
       if(time == 0){
        clearInterval(timer)
        btn.disable = false;
        btn.innerHTML = '发送'
        time = 10
    }
       btn.innerHTML = '还剩下' + time + '秒';
       time--;
       },1000)
})
~~~

#### this说明

+ 全局作用下this指向window
+ 定时器的this也是指向window
+ 方法里面的this指向它的调用者
+ 构造函数中的this指向是实例对象

### JS执行机制

+ JS是单线程，同一时间只能做一件事
+ 同步需要等待，异步可以多线程
+ 同步任务在主线程，异步是通过回调函数实现的，放入了消息队列中
+ 先执行同步任务，异步任务先放入队列，等同步任务执行完了再执行
+ 异步任务有三种类型，如：
  + 普通时间，如click、resize
  + 资源加载，如load、error
  + 定时器，包括setInterval、setTimeout
+ 异步处理器负责把异步任务放入任务队列，如不点击就不把执行函数放入任务队列
+ 事件循环是主线程不停的循环查看异步任务队列是否需要执行

### location对象页面跳转

URL是统一资源定位符，是互联网上标准资源的地址。一般格式为：

`protocol://host[:port]/path/[?query]#fragment`

+ `protocol`是通信协议
+ host是主机域名
+ port是端口号，http的默认端口为80
+ path路径，由/符号隔开
+ query参数，以键值对的形式通过&分开
+ fragment 片段 #后面内容 常见于链接和锚点

#### location对象的属性

| location对象的属性 | 返回值             |
| ------------------ | ------------------ |
| location.href      | 获取或者设置URL    |
| location.host      | 返回主机（域名）   |
| location.port      | 返回端口号         |
| location.pathnme   | 返回路径           |
| location.search    | 返回参数           |
| location.hash      | 返回片段 #后面内容 |

案例：五秒后自动跳转页面

~~~js
var btn = docunment.querySelector('button')
var div = docunment.querySelector('div')
btn.addEventListener('click',function(){
    location.href = "url"
})
var time = 5
setInterval(function(){
    if(time == 0){
         location.href = "url"
    }
    div.innerHTML = time + "秒之后跳转到首页";
    time --;
},1000)
~~~

#### 页面之间的传值

通过把url中的参数截取出来实现

~~~js
location.search //?uname=andy
// 1.去掉？ substr
var params = location.search.substr(1);
var arr = params.splite('=')[1]
div.innerHTML = arr
~~~

| location对象的方法 | 返回值                                        |
| ------------------ | --------------------------------------------- |
| location.assign()  | 跟href一样可以跳转页面，可以后退              |
| location.replace() | 替换当前页面，不能后退                        |
| location.reload()  | 重载页面，相当于刷新页面 参数为true则强制刷新 |

### navigator对象及其属性

navigator对象包含浏览器相关信息，最常用的是userAgent,该属性可以返回由客户机发送服务器的user-agent头部的值，可以判断是否为移动端还是PC端

`if((navigator.userAgent.match(/(phone|pad|pod......))))`

### history实现页面刷新

与浏览器的历史进行交互

| history对象方法 | 作用                                    |
| --------------- | --------------------------------------- |
| back()          | 后退                                    |
| forward（）     | 前进                                    |
| go(参数)        | 前进后退功能，1就是前进一个-1是后退一个 |
