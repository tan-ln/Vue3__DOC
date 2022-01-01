# 过渡和动画
> 过渡是指从一个状态到另一个状态，颜色。。
> 元素运动的情况

## <transition></transition>
`transition` 组件配合 css
```css
  .v-enter-from {
    opacity: 0;
  }
  .v-enter-active {
    transition: opacity 3s ease-in-out;
  }
  .v-enter-to {
    opacity: 1;
  }
  .v-leave-from {
    opacity: 1;
  }
  .v-leave-active {
    transition: opacity 3s ease-out;
  }
  .v-leave-to {
    opacity: 0;
  }
```
```js
<transition>
  <div v-if="show">hello world</div>
</transition>
```

## 配合 animation
```css
  @keyframes leftToRight {
    0% {
      transform: translateX(-100px);
    }
    50% {
      transform: translateX(-50px);
    }
    100% {
      transform: translateX(50px);
    }
  }

  .v-enter-active {
    animation: leftToRight 1s ease-out;
  }
```


## 自定义
1. name

```html
<style>
  /* 名称对应修改 */
  .formShow-enter-from {
    ...
  }
</style>

<transition name="formShow"></transition>
```
2. 属性名
```html
<style>
.enterClass {
  animation: leftToRight 1s ease-out;
}
</style>

<transition
  enter-from-class=""
  enter-to-class=""
  enter-active-class="enterClass" 
  leave-active-class="leaveClass"
></transition>
```

3. 引入 animation.css
```html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css" />


<transition
  enter-active-class="animate__animated animate__bounce"
  enter-leave-class="animate__animated animate__bounce"
></transition>
```

4. type
> 当动画和过渡*结合*，并且二者*时间不一致*时，通过 `type` 属性确认以哪一个为准，时间一到，动画直接结束
```html
<transition type="transition"></transition>
```

5. duration
> 自定义 时间，css 中定义的时间失效
```html
<transition :duration="1000"></transition>
<transition 
  :duration="{
    enter: 1000,
    leave: 2000
  }"
></transition>
```

6. :css="false"
> 不通过 css 设置效果，而是通过 js
```js
methods: {
  handleBeforeEnter(el) {
    el.style.color = 'red'
  },
  handleEnterActive(el, done) {
    const animator = setInterval(() => {
      const color = el.style.color
      el.style.color = (color === 'red') ? 'green' : 'red'
    }, 1000)

    setTimeout(() => {
      clearInterval(animator)
      done()                  // 动画结束后 调用，handleEnterEnd才有效
    }, 3000)
  },
  handleEnterEnd() {}
},

template: `
  <transition
    :css="false"
    @before-enter="handleBeforeEnter"
    @enter="handleEnterActive"
    @after-enter="handleEnterEnd"
  ></transition>
`
```

7. mode="out-in"
> 当切换动画效果同时进行时，可以选择先执行哪一个
```html
<!-- 先 out 再 in -->
<transition mode="out-in"></transition>
```

8. appear
> 初次显示携带动画
```html
<transition mode="out-in" appear></transition>
```

## transition-group
```css
.v-move {
  transition: all .5s ease-in;
}
```


























