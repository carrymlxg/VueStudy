## 组件基础
 
### data 必须为函数
因为每一个组件都是一个vue实例，，通过new vue() 实例化，引用同一个对象
如果data为对象，那么一旦其中一个组件的值被修改，另一个组件的值也会被修改


