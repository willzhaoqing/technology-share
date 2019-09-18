## call
call 方法第一个参数是要绑定给this的值，后面传入的是一个参数列表。当第一个参数为null、undefined的时候，默认指向window。
```
obj1.fn()
obj1.fn.call(obj1)

fn1()
fn1.call(null)

f1(f2)
f1.call(null,f2)
```
## apply
apply接受两个参数，第一个参数是要绑定给this的值，第二个参数是一个参数数组。当第一个参数为null、undefined的时候，默认指向window。
```
obj1.fn() 
obj1.fn.apply(obj1)

fn1()
fn1.apply(null)

f1(f2)
f1.apply(null,f2)
```
## bind
和call很相似，第一个参数是this的指向，从第二个参数开始是接收的参数列表。区别在于bind方法返回值是函数以及bind接收的参数列表的使用。
```
// bind返回值是函数
var obj = {
    name: 'Dot'
}

function printName() {
    console.log(this.name)
}

var dot = printName.bind(obj)
console.log(dot) // function () { … }
dot()  // Dot
```
bind 方法不会立即执行，而是返回一个改变了上下文 this 后的函数。而原函数 printName 中的 this 并没有被改变，依旧指向全局对象 window。
## 应用场景
1. 求数组中的最大和最小值
```
var arr = [1,2,3,89,46]
var max = Math.max.apply(null,arr)//89
var min = Math.min.apply(null,arr)//1
```
2. 将类数组转化为数组
```
var trueArr = Array.prototype.slice.call(arrayLike)
```
3. 数组追加
```
var arr1 = [1,2,3];
var arr2 = [4,5,6];
var total = [].push.apply(arr1, arr2);//6
// arr1 [1, 2, 3, 4, 5, 6]
// arr2 [4,5,6]
```
4. 判断变量类型
```
function isArray(obj){
    return Object.prototype.toString.call(obj) == '[object Array]';
}
isArray([]) // true
isArray('dot') // false
```
5. 利用call和apply做继承
```
function Person(name,age){
    // 这里的this都指向实例
    this.name = name
    this.age = age
    this.sayAge = function(){
        console.log(this.age)
    }
}
function Female(){
    Person.apply(this,arguments)//将父元素所有方法在这里执行一遍就继承了
}
var dot = new Female('Dot',2)
// 使用 log 代理 console.log
function log(){
  console.log.apply(console, arguments);
}
// 当然也有更方便的 var log = console.log()
```
