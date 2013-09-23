Step.js
=======

Node.js 控制流程工具。解决Node.js读文件、查询数据库等回调函数相互依赖，或者分别获取内容最后组合数据返回等应用情景。

## Example 使用示例

回调函数顺序依赖、依次执行。适用于下一个回调函数依赖于上一个函数执行的结果。

使用方法：`Step.Step(func,[func,[func,[...]]]);`，函数参数为任意数量的回调函数。也可以是一个类、函数数组或者它们的组合。

当上一个回调执行完成返回__非__`undefined`的结果时，或者在回调内部中调用`this.step()`时，表示这个回调执行完成，
并把其结果作为下一个回调的第一个参数，把所有执行结果按次序组成的的数组作为下一个回调的第二个参数。

示例代码：


```javascript
var Step = require('Step.js');

Step.Step(function(result,entire){

  console.log(result); // undefined
  console.log(entire); // []
  var that =this;
  setTimeout(function(){
    that.step('abc');
  },1000);
  
},function(result,entire){

  console.log(result); // 'abc' 
  console.log(entire); // ['abc']  
  return 123; //return ::返回一个非undefined的值，和调用this.step();效果相同
  
},function(result,entire){
 
  console.log(result); // 'abc'  ::此值为上一个回调执行的结果
  console.log(entire); // ['abc', 123]  ::此值为历史所有回调执行结果按次序组成的数组
  var that=this;
  setTimeout(function(){
    that.step({abc:123});
    console.log(that.index); //that.index为一个整数，代表回调被调用的次序，而不是返回结果的次序。
  },200);
  
},function(result,entire){

  console.log(result); // {abc:123}
  console.log(entire); // [ 'abc', 123, { abc: 123 } ]
  
});
```
