# jQuery - Selector 选择器

## 目录




<br><br><br><br><br><br>
### 选择器使用的性能优先级

个人总结选择器的性能优先级如下表
1. **id** - `$('#youId')`
1. **parent > children** - `$('#div > span')` 层级选择，显式指定明确层级
1. **parent children** - `$('#div p')` 子集查找，在父集里查找所有匹配表达式的所有元素，层级不限
1. **tag.className** - `input.myInput` 指定标签类型再指定子元素的查找表达式

### 尝试使用子集查找

