# js类型
js类型有七种，分别是
* Undefined
* Null
* Boolean
* String
* Number
* Symbol
* Object

 其中前6中是**基本数据类型**，Object是**引用类型**

## 基本类型和引用类型区别（参考js高级程序设计）
> 基本类型是存放在栈内存中，引用类型是存在堆内存中的。

#### 基本类型赋值
```
var num1 = 5
var num2 = num1
```
在内存中的存储如下图，num1 和 num2 是相互独立的，两个值参与任何操作不会相互影响

![](//p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/efac5c332a254cef99bcba5aca969c66~tplv-k3u1fbpfcp-zoom-1.image)

#### 引用类型赋值
当从一个变量向另一个变量复制引用类型的值时，同样也会将存储在变量对象中的值复制一份放到
为新变量分配的空间中。不同的是，这个值的副本实际上是一个指针，而这个指针指向存储在堆中的一
个对象。复制操作结束后，两个变量实际上将引用同一个对象。因此，改变其中一个变量，就会影响另
一个变量。

```
var obj1 = new Object(); 
var obj2 = obj1; 
obj1.name = "Nicholas"; 
alert(obj2.name); //"Nicholas"
```
内存展示如图，obj1 和ojb2 指向同一对象，

![](//p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/48977e890d9147b6bce02233f9cf634a~tplv-k3u1fbpfcp-zoom-1.image)
## null 和 undefined
### undefined
> 在使用 var 声明变量但未对其加以初始化时，
这个变量的值就是 undefined
#### 判断
```
var message; 
console.log(message == undefined); //true
```
我们平常都是这样去判断的，但是这样会有个问题，如果message没有被定义呢，

```
console.log(message == undefined); //产生错误
```
这个时候我们就需要换一种方式去判断了，那用什么呢？

```
console.log(typeof message == undefined); //产生错误
```
用typeof去判断，不管是未初始化还是未定义都返回undefined，但是，这样也会存在问题，就是你不知道这个变量是否定义过，那就需要我们自己去定义个规则，定义变量必须要初始化，这样当**typeof**操作符返回"undefined"时，我们就知道这个变量没有被声明。

我以为我学到这里就很了解undefined了，然鹅
看了winter老师的重学前端-类型里面讲，**“因为 JavaScript 的代码 undefined 是一个变量，而并非是一个关键字，这是 JavaScript 语言公认的设计失误之一，所以，我们为了避免无意中被篡改，我建议使用 void 0 来获取 undefined 值”**
基础薄弱的我，蒙了，还能篡改，是个变量，咦，我去查查资料吧。
查完了，接下来开始我的表演。（刚开始学winter老师的重学前端，我开始觉得写的一点也不详细，看不懂，写的好差，现在想想，是我low了）

那undefined是一个变量？答案是的。
> mdn描述：undefined是全局对象的一个属性。也就是说，它是全局作用域的一个变量。undefined的最初值就是原始数据类型undefined。

```
console.log(window.undefined) // undefined
console.log(typeof window.undefined) // undefined
```
😯 看来是变量，那么，undefined是个变量，那我就可以去改变undefined的值了
```
var undefined = 'a'
console.log(undefined) // undefined
console.log(typeof undefined) // undefined
```
咦，不可以呢，为啥，因为mdn说了 **“在现代浏览器（JavaScript 1.8.5/Firefox 4+），自ECMAscript5标准以来undefined是一个不能被配置（non-configurable），不能被重写（non-writable）的属性。即便事实并非如此，也要避免去重写它。”**

这个属性被配置成不可重写了，但是是在现代浏览器下。那现代浏览器下我们就可以放心大胆用undefined了吗?并不是，在局部作用域中，undefined是可以被修改的。

```
(function() {
    var undefined = 'foo';
    console.log(undefined, typeof undefined) //foo string
    let data
    <!--这个时候判断就会有问题了-->
    if(data==undefined){
    
    }
})()
```
那怎么解决这个问题呢，我该用什么去判断是不是undefined呢？
答案是用void 0
> 就是使用void对后面的表达式求值，无论结果是多少，都会返回原始值undefined。

```
console.log(void 0)
console.log(typeof data== void 0)
```

我终于明白winter老师说的啥意思了😭

### null 
null 就简单了，有一点注意，他的类型是object，null一般是指是一个空的对象
### null 和undefined区别

```
 console.log(null == undefined); //true 因为这里会做一个类型转换
  console.log(null === undefined); //false 类型不同
```


# 如何判断类型
* typeof
    > 返回一个字符串，表示未经计算的操作数的类型。


    typeof 除了Object 类型 其他类型都能识别（可识别的类型有：number，string，object，boolean，symbol，object，function）
    <br>
    eg:
    ```
    let a
    let b = 2
    let c = '2'
    let d = null
    let e = true
    let f = Symbol()
    let g = {name:'xxx'}
    let h = function(){}
    
    console.log(typeof a)           //undefined
    console.log(typeof b)           //number
    console.log(typeof c)           //string
    console.log(typeof d)           //object
    console.log(typeof e)           //boolean
    console.log(typeof f)           //symbol
    console.log(typeof g)           //object
    console.log(typeof h)           //function
    
    ```

* instanceof
    > 用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上 ()


    ```
    console.log([1, 2] instanceof Array) true
    
    function Foo(name) {
        this.name = name
    }
    var foo = new Foo('bar')
    console.log(foo instanceof Foo) // true
    ```
* Object.prototype.toString.call()
> toString方法的作用是返回一个对象的字符串形式，默认情况下返回类型字符串。
```
var a = new Object()
a.toString() // "[object Object]"

//而其他对象分别部署了自定义的toString方法
例如
[1,2].toString() // "1,2,3"
```
所以我们可以使用object的toString判断数据类型
```
Object.prototype.toString.call(2) //[object Number]
Object.prototype.toString.call(true) //[object String]
```
还可以封装为方法
```
var type = function (o){
  var s = Object.prototype.toString.call(o);
  return s.match(/\[object (.*?)\]/)[1].toLowerCase();
};
type(a) //[object Number] //number
```
# 参考文献
* mdn
* 《JavaScript高级程序设计》
* https://juejin.im/post/6844903574242066440#heading-6

<!--  hasOwnProperty-->
<!--判断属性是不是对象本身的属性-->
<!--```-->
<!--var item-->
<!--for (item in f) {-->
<!--    // 高级浏览器已经在 for in 中屏蔽了来自原型的属性，但是这里建议大家还是加上这个判断，保证程序的健壮性-->
<!--    if (f.hasOwnProperty(item)) {-->
<!--        console.log(item)-->
<!--    }-->
<!--}-->
<!--```-->

