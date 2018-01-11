## ES6 Practices


> #### Generator函数
 
 * ##### Generator 简介
 	- 执行 Generator 函数会返回一个遍历器对象，也就是说，Generator 函数除了状态机，还是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历 Generator 函数内部的每一个状态

 *  ##### for...of 循环 
 	- 遍历迭代对象  more:遍历迭代对象方法 for..of (...) Array.from() function * f()

 * ##### Generator.prototype.throw() 
 	- Generator函数返回的遍历器对象，都有一个throw方法,可以在函数体外，抛出错误，然后在Generator函数体内捕获

 * ##### Generator.prototype.return() 
 	- 终止指针往下移动,value等于参数值,如果没有参数，value则是undefined
 	- 如果Generator函数内部有try...finally代码块，那么return方法会推迟到finally代码块执行完再执行 

 * ##### next()、throw()、return() 的共同点 
 	- 它们的作用都是让 Generator  函数恢复执行，并且使用不同的语句替换yield表达式

 * ##### yield * 表达式 
 	- 在Generator函数内部，调用另一个Generator函数
 	- 定义 ： 从语法角度看，如果yield表达式后面跟的是一个遍历器对象，需要在yield表达式后面加上星号，表明它返回的是一个遍历器对象。这被称为 yield * 表达式
 	- 任何数据结构只要有Iterator接口( 字符串、数组、Generator函数等 )，就可以被 yield * 遍历
 	- 嵌套数组遍历
 	- 遍历完全二叉树

 * ##### 作为对象属性的Generator函数

```js
    let obj = {
      * myGeneratorMethod() {
        ···
      }
    };
    
    let obj = {
      myGeneratorMethod: function* () {
        // ···
      }
    };
```

 * ##### Generator函数的this
     - 因为Generator函数返回的总是遍历器对象，而不是this对象
     - Generator函数也不能跟new命令一起用,会报错
     - 有没有办法让 Generator 函数返回一个正常的对象实例，既可以用next方法，又可以获得正常的this？
      -- 首先，生成一个空对象，使用call方法绑定 Generator 函数内部的this。这样，构造函数调用以后，这个空对象就是 Generator 函数的实例对象了
```js
    function * F(){
    	this.a = 1;
    	yield this.b = 2;
    	yield this.c = 3;
    }
    
    var obj = {};
    var f = F.call(obj);
    
    console.log(f.next()); //{value: 2, done: false}
    console.log(f.next()); //{value: 3, done: false}
    console.log(f.next()); //{value: undefined, done: true}
    
    console.log(obj.a); // 1
    console.log(obj.b); // 2
    console.log(obj.c); // 3
```
 * ##### 含义和应用

 	- Generator是实现状态机的最佳结构,每运行一次，就改变一次状态
 	- Generator之所以可以不用外部变量保存状态，是因为它本身就包含了一个状态信息，即目前是否处于暂停态
	- Generator可以暂停函数执行，返回任意表达式的值。这种特点使得Generator 有多种应用场景
	- 处理异步操作，改写回调函数
```js
    var clock = function* () {
      while (true) {
        console.log('Tick!');
        yield;
        console.log('Tock!');
        yield;
      }
    };
```


```js
    function* loadUI()
    {
      showLoadingScreen();
      yield loadUIDataAsynchronously();
      hideLoadingScreen();
    }
    
    var loader = loadUI();
    // 加载UI
    loader.next()
    
    // 卸载UI
    loader.next()
```	


