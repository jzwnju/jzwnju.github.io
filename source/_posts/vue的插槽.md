---
title: vue的插槽
categories: 前端
tags: vue
date: 2019-12-12 21:36:24
thumbnail: '../assets/img/thumbnails/2.jpg'
---


## 1. v-slot
vue插槽有两种：具名插槽和作用域插槽，2.6.0以后，使用新语法v-slot指令（缩写为#）将两种语法统一起来。

老旧的语法，这里不再赘述，参见vue官网。下面讲讲新语法的用法。

<!-- more --> 

```html
<!-- 后备内容显示用户的名，以取代正常情况下用户的姓 -->

<!-- 父组件  -->
<!-- 要用template包裹，slot名称为slotName（具名插槽），放入对应名称的插槽位置(子组件未给name属性，父组件写default）； -->
  <!-- 接收子组件传入的属性传入属性，并命名为slotProps（作用域插槽），父级作用域中就可以使用子组件中传入的属性。（这里父级接收时可以使用解构语法） -->
<current-user>
  <template v-slot:slotName="slotProps">
    {{ slotProps.user.firstName }}
  </template>
</current-user>
```

```html
<!-- 子组件current-user中 -->
<span>
  <slot name="slotName" v-bind:user="user">
    <!-- 父组件未传入插槽内容时，使用这里的默认内容 -->
    {{ user.lastName }}
  </slot>
</span>
```

***

## 2. v-slot缩写
1. 跟 v-on 和 v-bind 一样，v-slot 也有缩写，即把参数之前的所有内容 (v-slot:) 替换为字符 #。例如 v-slot:header 可以被重写为 #header
2. 你希望使用缩写的话，你必须始终以明确插槽名取而代之。即使子组件未给slot明确的name属性。
```html
<current-user #default="{ user }">
  {{ user.firstName }}
</current-user>
```

```html
<span>
  <slot v-bind:user="user"></slot>
    {{ user.lastName }}
  </slot>
</span>
```

***

## 3. 动态插槽名
动态指令参数也可以用在 v-slot 上，来定义动态的插槽名：
```html
<base-layout>
  <template v-slot:[dynamicSlotName]>
    ...
  </template>
</base-layout>
```
