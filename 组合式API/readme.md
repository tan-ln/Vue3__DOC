# setup
```js
// 外部可以调用 setup 方法
methods: {
  test() {
    // 方法 setup 的返回值
    console.log(this.$options.setup())
  }
},

mounted() {
  this.test()
},
// created 实例完全被初始化之前
// 没有 $el，不能使用 this
setup(props, context) {
  // 返回的内容 暴露在外部
  return {
    name: 'tang',
    age: 12,
    handleClick: () => {
      console.log('123')
    }
  }
}
```

- `context` 参数
```js
  setup(props, context) {
    const { h } = Vue
    // attrs: Non-Props 属性 --> 父组件传递的属性子组件不接收
    const { attrs, slots, emit } = context
    // console.log(attrs)      // Proxy {app: "app", __vInternal: 1}
    // console.log(slots)        // Proxy {_: 1, __vInternal: 1, default: ƒ}
    // console.log(slots.default())  // [] 虚拟 dom
    // console.log(emit)        // 向外触发事件 emit('change'), 等价于methods 里的 this.$emit('change')
    
    // 调用渲染函数
    // 如果返回了渲染函数，则不能再返回其他 property
    // 可以通过 父组件的 ref 或 expose({ ... })
      return () => h('div', {}, slots.default())
  }
```


## 生命周期钩子
> beforeCreated 和 created 使用 setup()

```js
  setup(props, context) {
    // mounted 
    onMounted(() => {
      console.log('mounted!')
    })
    // updated 
    onUpdated(() => {
      console.log('updated!')
    })
    onUnmounted(() => {
      console.log('unmounted!')
    })
  }
```

```js
beforeMount -> onBeforeMount

mounted -> onMounted

beforeUpdate -> onBeforeUpdate

updated -> onUpdated

beforeUnmount -> onBeforeUnmount

unmounted -> onUnmounted

errorCaptured -> onErrorCaptured

renderTracked -> onRenderTracked

renderTriggered -> onRenderTriggered

activated -> onActivated

deactivated -> onDeactivated
```
