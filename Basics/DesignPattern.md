# JS DesighPattern
## Singleton
用来划分命名空间。减少全局变量的数量。多人开发时避免代码冲突。在中小型项目或者功能中，单体可以用作命名空间把自己的代码组织在一个全局变量名下；在稍大或者复杂的功能中，单体可以用来把相关代码组织在一起以便日后好维护。
*使用单体的方法就是用一个命名空间包含自己的所有代码的全局对象*
<code><pre>　var functionGroup  =newfunction myGroup(){
 　　　　this.name ='Tommy';
 　　　　this.getName =function(){
 　　　　　　returnthis.name
 　　　　}
 　　　　this.method1 =function(){}
 　　　　...
 　　}</pre></code>

## Factory
*提供一个创建一系列相关或相互依赖对象的接口，而无需指定他们具体的类。*
工厂就是把成员对象的创建工作转交给一个外部对象，好处在于消除对象之间的耦合。通过使用工厂方法而不是new关键字及具体类，可以把所有实例化的代码都集中在一个位置，有助于创建模块化的代码，这才是工厂模式的目的和优势。
<code><pre> var XMLHttpFactory =function(){};　　　　　　 //这是一个简单工厂模式
  　　XMLHttpFactory.createXMLHttp =function(){
 　　　　 var XMLHttp = null;
  　　　　if (window.XMLHttpRequest){
  　　　　　　XMLHttp = new XMLHttpRequest()
 　　　　 }elseif (window.ActiveXObject){
  　　　　　　XMLHttp = new ActiveXObject("Microsoft.XMLHTTP")
  　　　　}
 　　return XMLHttp;
 　　}
 　　//XMLHttpFactory.createXMLHttp()这个方法根据当前环境的具体情况返回一个XHR对象。
 　　var AjaxHander =function(){
 　　　　var XMLHttp = XMLHttpFactory.createXMLHttp();
 　　　　...
 　　}</pre></code>
## Decorator
这个模式装饰者模式用于在不修改现有对象或从派生子类的前提下为其添加方法。装饰者的运作过程是透明的，这就是说你可以用它包装其他对象，然后继续按之前使用那么对象的方法来使用。
<code><pre> 1 　　//创建一个命名空间为myText.Decorations
 2 　　var myText= {};
 3 　　myText.Decorations={};
 4 　　myText.Core=function(myString){
 5    　　this.show =function(){return myString;}
 6 　　}
 7 　　//第一次装饰
 8 　　myText.Decorations.addQuestuibMark =function(myString){
 9    　　this.show =function(){return myString.show()+'?';};
10 　　}
11 　　//第二次装饰
12 　　myText.Decorations.makeItalic =function(myString){
13    　　this.show =function(){return'<li>'+myString.show()+'</li>'};
14 　　}
15 　　//得到myText.Core的实例
16 　　var theString =new myText.Core('this is a sample test String');
17 　　alert(theString.show());　　//output 'this is a sample test String'
18 　　theString =new myText.Decorations.addQuestuibMark(theString);
19 　　alert(theString.show());　　//output 'this is a sample test String?'
20 　　theString =new myText.Decorations.makeItalic (theString);
21 　　alert(theString.show());　　//output '<li>this is a sample test String</li>'</pre></pre>
从这个示例中可以看出，这一切都可以不用事先知道组件对象的接口，甚至可以动态的实现，在为现有对象增添特性这方面，装饰者模式有极大的灵活性。

　　如果需要为类增加特性或者方法，而从该类派生子类的解决办法并不实际的话，就应该使用装饰者模式。派生子类之所以会不实际最常见的原因是需要添加的特性或方法的数量要求使用大量子类。
## Composite 组合模式
将对象组合成树形结构以表示“部分-整体”的层次结构。它使得客户对单个对象和复合对象的使用具有一致性。
