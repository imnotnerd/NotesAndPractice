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
将对象组合成树形结构以表示“部分-整体”的层次结构。它使得客户对单个对象和复合对象的使用具有一致性。可以用来创建Web上的动态用户界面而量身定制的模式。使用这种模式，可以用一条命令在多个对象上激发复杂的或递归的行为。组合模式擅长于对大批对象进行操作。
* * *
好处：1.可以用同样的方法处理对象的集合与其中的特定子对象；2.可以用来把一批子对象组织成树形结构，并且使整棵树都可被便利。
* * *

组合模式适用范围：1.存在一批组织成某处层次体系的对象（具体结构可能在开发期间无法知道）；2.希望对这批对象或其中的一部分对象实话一个操作。
## Facade模式 ：门面模式是几乎所有JavaScript库的核心原则

子系统中的一组接口提供一个一致的界面，门面模式定义了一个高层接口，这个接口使得这一子系统更加容易使用，简单的说这是一种组织性的模式，它可以用来修改类和对象的接口，使其更便于使用。

门面模式的两个作用：1.简化类的接口；2.消除类与使用它的客户代码之间的耦合。

在处理游览器差异问题时最好的解决办法就是把这些差异抽取的门面方法中，这样可以提供一个更一致的接口，addEvent函数就是一个例子。

<code><pre>  　　_model.util是一个命名空间
  　　_myModel.util.Event = {
     　　getEvent:function(e){
        　　return e|| window.event;
     　　},
     　　getTarget:function(e){
        　　return e.target||e.srcElement;
    　　},
  　　preventDefault:function(e){
       　　if(e.preventDefault){
         　　 e.preventDefault();
       　　}else{
          　　e.returnValue =false;
       　　}
    　　}
 　　};
 　　//事件工具大概就是这么一个套路，然后结合addEvent函数使用
 　　addEvent(document.getElementsByTagName('body')[0],'click',function(e){
    　　alert(_myModel.util.Event.getTarget(e));
　　});
