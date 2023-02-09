---
title: 使用Sortable.js和Vue.Draggable的一些坑
date: 2018-05-17 17:55:47
tags:
- vue
- javascript
---
Sortable.js 是拖拽列表最著名的库，而 Vue.Draggable 是其在 Vue 中的封装，在 vue 中引入十分的方便。

但是我在项目中的需求是左侧列表初始时是空的，从右侧列表拖动到左侧生成，这就造成了一个问题，左侧因为没有dom节点而拖不过来，这时就需要在左侧的 draggable 组件添加一个 min-height 的 css 属性。

另一个问题是，在使用 clone 模式时，因为 js 对象都是指向同一个指针的，再从右侧拖动到左侧的节点，当修改其属性时，把所有拖拽过来的相同节点也都修改了。  
我的解决方法是，把右侧改为 pull 模式，同时使用 vue 的 watch，当右侧的列表发生变化时重新生成，这样就和初始化的对象完全脱离了。

