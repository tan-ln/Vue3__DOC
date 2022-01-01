# 自定义指令

## 全局指令
```html
<div v-focus="top"></html>
```
```js
  app.directive('focus', {
    beforeMount(el) {},
    mounted(el) {
      el.focus()
    },
    beforeUpdate() {},
    updated() {},
    beforeUnmount() {},
    unmounted() {}
  })
``` 

## 局部指令
```js
  const directives = {
      focus: {
        mounted(el) {
        el.focus()
      }
    }
  }

  const app = Vue.createApp({
    directives,                     // 调用
    data() {}
  })
```

## 传值
> `binding.value`

```html
<div v-pos="top"></html>
```
```js
  app.directive('pos', {
    mounted(el, binding) {
      el.style.top = binding.value
    },
    updated(el, binding) {
      el.style.top = binding.value
    }
  })

```
> 等价于 (在 mounted 和 updated 内容相同的情况下)
```js
  app.directive('pos', (el, binding) => {
      el.style.top = binding.value
  })

```

> `binding.arg = left`
> 可以通过此参数，灵活控制自定义指令
```html
<div v-pos:left="top"></html>
```
