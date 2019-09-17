# 事件
事件是用来实现js和html之间交互的，可以用侦听器（或处理程序）来预订事件，以便事件发生时执行相应的代码。这种在传统软件工程中被称为观察员模式的模型，支持页面的行为（js）与页面的外观（html和css）的松散耦合。事件最早是在IE3和Netscape Navigator2中出现的，当时是作为分担服务器运算负载的一种手段。

## 一、事件流
事件流描述的是从页面中接收事件的顺序。IE的事件流是事件冒泡流，而Netscape Communicator 的事件流是事件捕获流。  
1. 事件冒泡：从触发的对象开始，事件不断往上传递。  
2. 事件捕获：从dom树一直向下传递事件直到捕获为止。  
3. DOM事件流：“DOM2级事件”规定的事件流包括三个阶段：事件捕获阶段、处于目标阶段和事件冒泡阶段。

## 二、事件绑定类型
1. dom0：通过element对象调用对应的事件属性绑定特定的事件，事件会在事件冒泡阶段被捕获。
```
let btn = document.getElementById("myBtn");
btn.onclick = function(){// 由于onclick是attr，所以可以通过.来获取
    console.log(this.id);    //"myBtn"
};
```

2. dom2：通过addEventListener来绑定事件、removeEventListener来移除事件。指定第三个参数为false的话，事件也是在冒泡阶段被触发。IE9、Firefox、Safari、Chrome 和 Opera 支持 DOM2 级事件处理程序。
```
let btn = document.getElementById("myBtn");
btn.addEventListener('click', function(event){
	console.log(this.id);    //"myBtn"
},false);
btn.addEventListener("click", function(){// 添加多个回调,触发的时候依次调用
    alert("Hello world!");
}, false);
```

3. 对于ie8之前的可以使用attachEvent添加事件、detachEvent解除事件。
```
var btn = document.getElementById("myBtn");
btn.attachEvent("onclick", function(){// 事件类型必须是onclick，跟dom2不一样
    alert("Clicked");
});
```
### 三者的区别

1. dom2、attachEvent可以为一个事件添加多个相应函数，彼此之间不会覆盖。dom0则不可以。
2. dom0的兼容性好，支持所有的浏览器。dom2不支持ie浏览器，所以对于ie浏览器事件处理使用attachEvent。由于IE只支持冒泡事件，所以attachEvent没有第三个参数
3. dom0在注销事件的时候，只要将对应的事件属性置为null即可。dom2调用removeEventListener函数的时候，参数要与addEventListener一样，也就是说，在addEventListener指定的函数的回调不能是一个匿名函数，不然在注销不到。因为两个函数对象是不一样的，虽然内容一样。
```
var btn = document.getElementById("myBtn");
var handler = function(){// 必须将回调函数显示声明
	alert(this.id);
};
btn.addEventListener("click", handler, false);
btn.removeEventListener("click", handler, false);// 将函数的引用传递进removeEventListener中才能注销回调
```
对于attachEvent回调函数中的this对象是window，dom0的回调函数是当前捕获事件的对象。
```
var btn = document.getElementById("myBtn");
btn.attachEvent("onclick", function(){
alert(this === window); //true
});
```
### 浏览器事件兼容性处理
```
var EventUtil = {
    addHandler: function(element, type, handler){
        if (element.addEventListener){
            element.addEventListener(type, handler, false);
        } else if (element.attachEvent){
            element.attachEvent("on" + type, handler);
        } else {
            element["on" + type] = handler;
        }
    },
    removeHandler: function(element, type, handler){
        if (element.removeEventListener){
            element.removeEventListener(type, handler, false);
        } else if (element.detachEvent){
            element.detachEvent("on" + type, handler);
        } else {
            element["on" + type] = null;
		} 
	},
	getEvent: function(event){
    	return event ? event : window.event;// ie中获取事件对象通过window.event
	},
	getTarget: function(event){
    	return event.target || event.srcElement;
	},
	preventDefault: function(event){
    	if (event.preventDefault){
			event.preventDefault();
		} else {
			event.returnValue = false;
		}
	},
	stopPropagation: function(event){
		if (event.stopPropagation){
            event.stopPropagation();
        } else {
            event.cancelBubble = true;
		}
	}
};
```
### 事件回调函数的作用域问题
    事件绑定函数时，该函数会以当前元素为作用域执行。

(1)使用匿名函数

    我们为回调函数包裹一层匿名函数。包裹之后，虽然匿名函数的作用域被指向事件触发元素，但执行的内容就像直接调用一样，不会影响其作用域。

(2)使用 bind 方法

    使用匿名函数是有缺陷的，每次调用都包裹进匿名函数里面，增加了冗余代码等，此外如果想使用 removeEventListener 解除绑定，还需要再创建一个函数引用。Function 类型提供了 bind 方法，可以为函数绑定作用域，无论函数在哪里调用，都不会改变它的作用域。

## 三、事件对象
如果将回调函数直接绑定到目标对象上，那么this、target、currentTarget都是一样的，都是目标对象
```
var btn = document.getElementById(“myBtn”);
btn.onclick = function(event){
console.log(event.currentTarget === this);
console.log(event.target === this);
};
```
如果是绑定到父级元素上，那么this 和 currentTarget 都等于父级元素，也即是回调函数被绑定的对象，target才是真正触发的对象。
```
document.body.onclick = function(event) {
console.log(event.currentTarget === document.body); // true
console.log(this === document.body); // true
console.log(event.target === document.getElementById(“myBtn”));// true
};
```
### document加载事件回调
+ DOMContentLoaded：在dom树加载完成之后被触发，不等待css、js、img等下载  
+ onload：等待页面的内容都加载完毕之后被触发
-----
中高级
-----

## 四、事件类型
    Web浏览器中可能发生的事件有很多类型。如前所述，不同的事件类型具有不同的信息，而 "DOM3级事件" 规定了下列几类事件：
    UI事件：在用户与页面上的元素交互时触发；
    鼠标事件：当用户通过鼠标在页面上执行操作时触发；
    滚轮事件：当使用鼠标滚轮（或类似设备）时触发；
    文本事件：当在文档中输入文本时触发；
    键盘事件：当用户通过键盘在页面上执行操作时触发；
    合成事件：当为IME（Input Method Editor,输入法编辑器）输入字符时触发；
    变动 (mutation) 事件：当底层 DOM 结构发生变化时触发。

1、UI事件
    (1)load事件：load可以用来判断图片加载完毕。注意：新创建的图像元素不是在加载到页面中才开始下载，而是设置src之后就开始下载。load可以用来判断js加载完成。<script>元素可以触发load事件，来判断动态加载的js文件是否加载完成。它和img不同，必须设置了src属性并且添加到文档之后才会开始下载。

    (2).unload事件

    (3).resize事件
    注意：关于何时会触发resize事件，不同浏览器有不同的机制。IE、Safari、Chrome和Opera会在浏览器窗口变化了1像素时就触发resize事件，然后随着变化不断重复触发。Firefox则只会在用户停止调整窗口大小时才会触发resize事件。
    (4).scroll事件
2、焦点事件
    焦点事件会在页面元素获得或失去焦点时触发，利用这些事件并与document.hasFocus()方法及document.activeElement属性配合，可以知晓用户在页面上的行踪。有以下6个焦点事件：blur、DOMFocusIn、DOMFocusOut、foucs、focusin、focusout。
3、鼠标与滚轮事件
    DOM3级事件定义了9个鼠标事件：click、dblclick、mousedown、mouseenter、mouseleave、mousemove、mouseout、mouseover、mouseup。
    鼠标事件中还有一类滚轮事件mousewheel事件。
    (1).客户区坐标位置：clientX和clientY属性。
    (2).页面坐标位置：pageX和pageY属性。
    (3).屏幕坐标位置：screenX和screenY属性。
    (4).修改键：shiftKey、ctrlKey、altKey、metaKey。
   (5).相关元素：DOM通过event对象的relatedTarget属性提供了相关元素的信息。而在IE中，mouseover事件触发时，fromElement保存了相关元素，mouseout事件触发时，toElement保存了相关元素。
    (6).鼠标按钮
    (7).更多的事件信息

    (8).鼠标滚轮事件：mousewheel事件中包含wheelDelta属性，Firefox支持类似的DOMMouseScroll事件，包含detail属性。

4、键盘与文本事件
    DOM3级含有3个键盘事件：keydown、keypress、keyup。
    1.键码：keyCode属性
    2.字符编码：charCode属性
    3.DOM3级变化：新属性（key、char、location属性：表示按下了什么位置上的键），getModifierState()方法。
    4.textinput事件：data属性：表示用户输入的字符（而非字符编码）、inputMethod属性：表示把文本输入到文本框中的方式。
5、复合事件：用以处理IME的输入序列的一类事件。
6、变动事件：删除节点、插入节点等。
7、HTML5事件
    1.contextmenu事件：用以表示何时应该显示上下文菜单。
    2.beforeunload事件：让开发人员有可能在页面卸载前阻止这一操作。
    3.DOMContentLoaded事件：在形成完整的DOM树之后触发。
    4.readystatechange事件：提供与文档或元素的加载状态有关的信息。
    5.pageshow和pagehide事件：pageshow事件在页面显示时触发，pagehide事件在浏览器卸载页面时触发。
    6.hashchange事件：以便在URL的参数列表发生变化时通知开发人员。
8、设备事件
    1.orientationchange事件
    2.MozOrientation事件
    3.deviceorientation事件
    4.devicemotion事件
9、触摸与手势事件
    1.触摸事件
    2.手势事件

五、内存和性能
1.事件委托
    事件委托可以解决页面中事件处理程序过多的问题。事件委托利用了事件冒泡，只指定一个事件处理程序，就可以处理某一类型的所有事件。
```
EventUtil.addHandler(document.body, 'click', function (event) {
  event = EventUtil.getEvent(event);
  var target = EventUtil.getTarget(event);
  switch (target.id) {
  case 'site_nav_top':
    alert('口号');
    break;
  case 'nav_menu':
    alert('点击菜单');
    EventUtil.preventDefault(event);
    break;
  case 'editor_pick_lnk':
    alert('推荐区');
    EventUtil.preventDefault(event);
    break;
  }
});
```
2.移除事件处理程序
    如果内存中保留大量无用的事件处理程序，会影响性能。所以一定要在不需要的时候及时移除事件处理程序。尤其注意以下情况：使用innerHTML删除带有事件处理程序的元素时，要先将事件处理程序设置为null。使用委托也可以解决这个问题，不直接将事件加载会被innerHTML替换的元素，而是将事件赋给其父元素，这样就可以避免了。卸载页面时，最好手工清除所有的事件处理程序。

六、DOM中的事件模拟
    事件经常由用户操作或通过其他浏览器功能来触发，也可以使用JS在任意时刻来触发特定的事件，而此时的事件就如同浏览器创建的一样。在测试Web应用程序，模拟触发事件是一种极其有用的技术。

1. DOM中的事件
    模拟分三步：

    使用document.createEvent()创建event对象。通过赋值不同的参数，可以模拟不同的事件类型
    初始化event对象；
    触发事件，调用dispatchEvent()方法，所有支持事件的DOM节点都可以支持这个方法。
```
function simulateClick() {
  var evt = document.createEvent("MouseEvents");
  evt.initMouseEvent("click", true, true, window,
    0, 0, 0, 0, 0, false, false, false, false, 0, null);
  var cb = document.getElementById("checkbox"); 
  var canceled = !cb.dispatchEvent(evt);
  if(canceled) {
    // A handler called preventDefault
    alert("canceled");
  } else {
    // None of the handlers called preventDefault
    alert("not canceled");
  }
}
```
2. 模拟鼠标事件
    首先createEvent()方法传入参数是“MouseEvent”来模拟鼠标事件。返回的event对象有一个initMouseEvent()方法，用来初始化事件信息。该方法有15个参数：
    type(字符串)：要触发的事件类型，比如“click”。
    bubbles(bool)：事件是否冒泡。一般设为true。
    cancelable(bool)：事件是否可以取消。一般设为true。
    view：与事件关联的视图，一般设置为document.defaultView。
    detail(整数)：与事件有关的详细信息，一般设置为0.
    screenX：事件相对于屏幕的X坐标。
    screenY：事件相对于屏幕的Y坐标。
    clientX：事件相对于视口的X坐标。
    clientY：事件相对于视口的Y坐标。
    ctrlKey(bool)：是否俺下了Ctrl键，默认为false。
    altKey：是否按下了alt键，默认false。
    shiftKey：是否按下了shift键，默认false。
    metaKey：是否按下了meta键，默认false.
    button(整数)：按下了哪个鼠标键，默认0。
    relatedTarget：与事件相关的对象，一般为null.只在模拟mouseover和mouseout的时候会用到。
    最后给DOM元素调用dispatchEvent()方法来触发事件。
```
var objmenu = document.getElementById('nav_menu');
objmenu.onclick = function () {
  console.log('menu');
}
document.body.onclick = function () {
  console.log('body');
}
var event = document.createEvent('MouseEvent');
event.initMouseEvent('click', true, true, document.defaultView, 0, 0, 0, 0, 0, false, false, false, false, 0, null);
for (var i = 0; i < 5; i++) {
  objmenu.dispatchEvent(event);
}
```
3. 自定义DOM事件
    DOM3中支持自定义DOM事件。要创建新的自定义事件，可以调用document.createEvent()方法，返回的对象有一个initCustomEvent()方法，包含四个参数：
    type：触发事件的类型；
    bubbles：事件是否冒泡；
    cancelable：事件是否可以取消；
    detail：任意值，保存在event.detail属性中。
    最后在DOM元素调用dispatchEvent()方法触发事件。
```
var objmenu = document.getElementById('nav_menu');
EventUtil.addHandler(objmenu, 'myevent', function (event) {
  console.log('menu' + event.detail);
});
EventUtil.addHandler(document.body, 'myevent', function (event) {
  console.log('body' + event.detail);
})
if (document.implementation.hasFeature('CustomEvents', '3.0')) {
  var event = document.createEvent('CustomEvent');
  event.initCustomEvent('myevent', true, true, '测试事件detail');
  for (var i = 0; i < 5; i++) {
    objmenu.dispatchEvent(event);
  }
}
```
自定义事件的函数有 Event、CustomEvent 和 dispatchEvent。直接自定义事件，使用 Event 构造函数；CustomEvent 可以创建一个更高度自定义事件，还可以附带一些数据，具体用法如下：

var myEvent = new CustomEvent(eventname, options);
其中 options 可以是：
```
{
    detail: {
        ...
    },
    bubbles: true,
    cancelable: false
}
```
    其中 detail 可以存放一些初始化的信息，可以在触发的时候调用。其他属性就是定义该事件是否具有冒泡等等功能。内置的事件会由浏览器根据某些操作进行触发，自定义的事件就需要人工触发。dispatchEvent 函数就是用来触发某个事件：
element.dispatchEvent(customEvent);
上面代码表示，在 element 上面触发 customEvent 这个事件。结合起来用就是：
```
// add an appropriate event listener
obj.addEventListener("cat", function(e) { process(e.detail) });
 
// create and dispatch the event
var event = new CustomEvent("cat", {"detail":{"hazcheeseburger":true}});
obj.dispatchEvent(event);
// 使用自定义事件需要注意兼容性问题，而使用 jQuery 就简单多了：

// 绑定自定义事件
$(element).on('myCustomEvent', function(){});
 
// 触发事件
$(element).trigger('myCustomEvent');
```
此外，你还可以在触发自定义事件时传递更多参数信息：
```
$( "p" ).on( "myCustomEvent", function( event, myName ) {
  $( this ).text( myName + ", hi there!" );
});
$( "button" ).click(function () {
  $( "p" ).trigger( "myCustomEvent", [ "John" ] );
});
```
