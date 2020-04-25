#### js中 style、currentStyle和getComputedStyle的区别
````javascript
// style 各大浏览器都兼容，能设置样式和获取样式，但是获取不了外部样式 ele.style.attr 
// currentStyle 只适用于IE，不兼容火狐和谷歌 ele.currentStyle.attr
// getComputedStyle 该属性兼容火狐和谷歌 不兼容IE8及以下 window.getComputedStyle(ele,null).attr
// getComputedStyle(oDiv,'::after').content

function getStyle(ele, attr) {
  if(ele.currentStyle) {
    return ele.currentStyle[attr];
  } else {
    return widnow.getComputedStyle(ele,fasle)[attr]
  }
}
````

#### 使用event对象
````javascript
// IE 中event为全局变量，不存在作用域的问题
// Chrome谷歌中 event并不是全局变量，他是在每个事件绑定的函数中都默认传入一个形参event
// 用户不传参
var a = arguments.callee.caller.arguments[0] || window.event;
````

#### 获取目标元素
````javascript
event.srcElement ? event.srcElement : event.target; // srcElement 兼容ie
````

#### attachEvent和addEventListener
````javascript
  //attachEvent IE的方法 不遵循W3C标准 后绑定先执行
  //addEventListener 主流浏览器都遵循的方法 先绑定先执行
  ele.attachEvent('onclick',function() {
    console.log('test ....')
  })  // IE11以及以上不支持 chrome不支持 ff不支持
  ele.addEventListener('click',function() {
    console.log('test1 ...')
  },false) // IE8 及 以下不支持
````

#### 获取dom节点
````javascript
// parentElement 获取对象层次中的父元素
// parentNode 获取文档层次中的父节点

````

#### 日期函数处理
````javascript
//IE8 以下
new Date().getFullYear() === new Date().getYear() // 2020
//IE9/标准浏览器 
new Date().getYear() // 120 得到的是当前年份（2020）与1900年的差值
````

#### 集合类对象问题
1. IE下，可以使用()或者[]获取集合类对象 
2. FireFox下，只能使用[] 获取集合类对象

#### 鼠标按键编码的兼容
````javascript
// W3C 标准下： 0 1 2 分别表示左 中 右
// IE10 以下：左中右 分别为1 4 2
function but(evt) {
  var e = evt || window.event;
  if(evt) {
    return e.button;
  } else if(window.event) {
    switch(e.button) {
      case 1: return 0;
      case 4: return 1;
      case 2: return 2
    }
  }
}
````

#### 鼠标当前坐标
1. IE : event.x 和 event.y
2. FF : event.pageX 和 event.pageY
3. 通用 event.clientX 和 event.clientY

#### 标签的x和y的坐标位置 
1. IE有style.posLeft 和 style.posTop FF:没有
2. 通用：object.offsetLeft 和 object.offsetTop

#### 窗体的高度和宽度
````javascript
// IE (页面一定要有body标签)
document.body.offsetWidth
document.body.offsetHeight
// FF
documents.innerWidth document.documentElement.clientWidth
documents.innerHeight document.documentElement.clientHeight
// 通用
document.body.clientWidth
document.body.clientHeight
````

#### 标签的自定义属性
IE：如果给标签div定义一个属性value,可以是div.value和div['value']取得该值
FF: 不能用div.value 和 div['value']
通用：div.getAttribute("value")


#### 兼容IE8 IE8 或者任意版本IE
1.大于 gt || 大于等于 gte || 小于 lt || 小于等于 lte(<!--[if gte IE 8]><![endif]-->)
````html
<!DOCTYPE html>
<!--[if IE 8] <html class="ie8" lang="en"><![endif]-->

<!--[if IE 9] <html class="ie9" lang="en"><![endif]-->

<!--[if (gt IE 9)|!(IE)]><!-->

<html lang="en"> <!--<![endif]>>
````

#### 对于360双核浏览器

1. 可以添加以下头部meta信息可以使得网页用webkit内核渲染：   
2. <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
3. IE=edge：保持使用最高级别模式显示内容；chrome=1：谷歌的外挂插件Google Chrome Frame（谷歌内嵌浏览器框架GCF），使用IE浏览网页时实际上是使用Chrome浏览器内核渲染，最低支持IE6，但前提是客户端已经安装GCF。
4. <meta name="renderer" content="webkit">

#### ie8中的一些css 不支持            
1. ie8支持:first-child,但不支持:last-child。因为前者是css2.1标准，后者是css3标准。推荐的做法不是使用last-child，而是给最后一个元素设置一个.last的class，然后对此进行样式设置，这样就全部兼容了。            
2. html5shiv.js IE8不支持HTML5的新标签，如<header>、<nav>等标签在IE8无法渲染。html5shiv.js可帮助IE6-8浏览器兼容HTML5语义化标签。使用方法：在页面中引用html5shiv.js文件。必须添加在页面的<head>元素内，因为IE浏览器必须在元素解析前知道这个元素，所以这个js文件不能在页面底部引用。           
3. Respond.js IE8不支持CSS媒体查询，对响应式设计大大不利。Respond.js可帮助IE6-8兼容“min/max-width”媒体查询条件。使用方法：在页面中所有css文件的引用位置之后引用Respond.js。而且Respond.js的引用得越早，用户看到页面闪烁的机会越小。           
4. CSS3字体单位“rem”兼容方案：rem.jsCSS3引入了新的字体大小单位rem，与em的“相对于其父元素来设置字体大小”的功能不同，rem是相对于根元素<html>的字体大小比率单位，成了目前主流的单位之一。IE9+开始支持，IE8就只能通过引入js库来支持了。使用方法：在页面中引用rem.js文件。需要引用在页脚，也就是<body>末尾，在所有css文件引用和DOM元素之后。           
5. 一些其它不支持的属性： border-radius 圆角box-shadow 盒子阴影CSS3 Background 背景渐变

#### placeholder 
1. 不支持ie10-以下的版本(可以通过使用一个span标签来模拟提示。)