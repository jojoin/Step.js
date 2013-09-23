Step.js
=======

Node.js 控制流程工具。解决Node.js读文件、查询数据库等回调函数相互依赖，或者分别获取内容最后组合数据返回等应用情景。

## Example 使用示例

回调函数顺序依赖、依次执行：

```javascript
var Step = require('Step.js');
Step.Step(function(ready,Data){
  var that =this;
  setTimeout(function(){
    that.step(['e','f','g']);
  },1000);
},function(ready,Data){
  return 'abc';
},function(ready,Data){
  var that=this;
  setTimeout(function(){
    that.step({abc:123});
    console.log(that.index); //that.index为一个整数，代表回调被调用的次序，而不是返回结果的次序。
  },200);
},function(ready,Data){
  console.log(Data); // [ [ 'e', 'f', 'g' ], 456, { abc: 123 } ]
});
```
