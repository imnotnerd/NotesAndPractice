## 深入浅出ES6 (二) ##  
* 使用内建的foreach方法来遍历数组：  
<code>myArray.forEach(function (value) {
console.log(value);
});</code>  
缺陷：你不能使用break语句中断循环，也不能使用return语句返回到外层函数。  
对比 for-in，for-in是为普通对象设计的，你可以遍历得到字符串类型的键，因此不适用于数组遍历。<code>
for (var index in myArray) { // 千万别这样做
  console.log(myArray[index]);
}</code>  
作用于数组的for-in循环体除了遍历数组元素外，还会遍历自定义属性。_ 举个例子，如果你的数组中有一个可枚举属性myArray.name，循环将额外执行一次，遍历到名为“name”的索引。就连数组原型链上的属性都能被访问到 _。
##### 强大的 for-of 循环 #####
<code>
for (var value of myArray) {
  console.log(value);
}</code>
优点：这个方法避开了for-in循环的所有缺陷。与forEach()不同的是，它可以正确响应break、continue和return语句。 _ for-in用来遍历对象属性，for-of遍历数据(例如数组)。还可以遍历DOM Nodelist和set以及map(ES6新增)。
* 迭代器对象
for-of循环首先调用集合的[Symbol.iterator]()方法，紧接着返回一个新的迭代器对象。迭代器对象可以是任意具有.next()方法的对象；for-of循环将重复调用这个方法，每次循环调用一次。以下是原文中提出的他能想到的最贱的迭代器：<code>
var zeroesForeverIterator = {
 [Symbol.iterator]: function () {
   return this;
  },
  next: function () {
  return {done: false, value: 0};
 }
};</code>    
迭代器对象也可以实现可选的.return()和.throw(exc)方法。如果for-of循环过早退出会调用.return()方法，异常、break语句或return语句均可触发过早退出。如果迭代器需要执行一些清洁或释放资源的操作，可以在.return()方法中实现。大多数迭代器方法无须实现这一方法。.throw(exc)方法的使用场景就更特殊了：for-of循环永远不会调用它。()  

## 深入浅出ES6 (三) ##   
* Generator  
   + 普通函数使用function声明，而生成器函数使用function*声明.  
   + 在生成器函数内部，有一种类似return的语法：关键字yield。二者的区别是，普通函数只可以return一次，而生成器函数可以yield多次（当然也可以只yield一次）。  
   + 在生成器的执行过程中，遇到yield表达式立即暂停，后续可恢复执行状态。

_ 这就是普通函数和生成器函数之间最大的区别，普通函数不能自暂停，生成器函数可以。_  
<pre><code>function* quips(name) {
  yield "你好 " + name + "!";
  yield "希望你能喜欢这篇介绍ES6的译文";
  if (name.startsWith("X")) {
    yield "你的名字 " + name + "  首字母是X，这很酷！";
  }
  yield "我们下次再见！";
}</code></pre>   
 调用quips生成器时发生了什么。  
 
 <pre><code> > var iter = quips("jorendorff");  
  [object Generator]  
    iter.next()
  { value: "你好 jorendorff!", done: false }
 iter.next()
  { value: "希望你能喜欢这篇介绍ES6的译文", done: false }
 iter.next()
  { value: "我们下次再见！", done: false }
 iter.next()
  { value: undefined, done: true }</code></pre>
当你调用一个生成器时，它并非立即执行，而是返回一个已暂停的生成器对象（上述实例代码中的iter）。你可将这个生成器对象视为一次函数调用，只不过立即冻结了，它恰好在生成器函数的最顶端的第一行代码之前冻结了。

每当你调用生成器对象的.next()方法时，函数调用将其自身解冻并一直运行到下一个yield表达式，再次暂停。
