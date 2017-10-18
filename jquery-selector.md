# jQuery - Selector 选择器

## 目录

- [选择器使用的性能优先级](#选择器使用的性能优先级)



<br><br><br><br><br><br>
### 选择器使用的性能优先级

个人总结选择器的性能优先级如下表
1. **id** - `$('#youId')`
1. **parent > children** - `$('#div > span')` 层级选择，显式指定明确层级
1. **parent children** - `$('#div p')` 子集查找，在父集里查找所有匹配表达式的所有元素，层级不限
1. **tag.className** - `$('input.myInput')` 指定标签类型再指定子元素的查找表达式
1. **.className** - `$('.myInput')` 样式选择器，性能最强，建议只在小范围中使用，例如已经获得了父容器的对象，在父容器的基础上再使用样式选择器，查找的范围可以被尽可能的缩小
  ```html
  <div id="myDiv">
    <input type="text" class="inputBox">
  </div>
  ```  
  js  
  ```js
  var parent = $('#myDiv');
  var input = $('.inputBox',parent);
  ```



### 尝试使用子集查找

