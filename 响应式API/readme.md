# 响应式 API

ref, reactive 响应式引用

原理：通过 proxy 对数据封装，当数据变化时，触发模板等内容更新


> `ref` 适合处理**基础类型**的数据
> `reactive` 处理非基础类型的数据

## ref
```js
setup(props, context) {
  const { ref } = Vue
  // proxy: 'tang' 变成 proxy({ value: 'tang' }) 这样的响应式引用
  // 并且 在渲染时会自动改写成 name.value
  let name = ref('tang')
  setTimeout(() => {
    name.value = '000'
  }, 1000)
  return {
    name
  }
}
```

## reactive
```js
// obj
setup(props, context) {
  const { reactive } = Vue
  const nameObj = reactive({ name: 'tang' })
  setTimeout(() => {
    nameObj.name = '000'
  }, 1000)
  return {
    nameObj
  }
}

// arr
setup(props, context) {
  const { reactive } = Vue
  const nameObj = reactive([123])
  setTimeout(() => {
    nameObj[0] = 456
  }, 1000)
  return {
    nameObj
  }
}
```

## readonly 对数据只读限制
> 将会报警告
```js
setup(props, context) {
  const { reactive, readonly } = Vue
  const nameObj = reactive([123])
  const copyNameObj = readonly(nameObj)
  setTimeout(() => {
    nameObj[0] = 456
    copyNameObj[0] = 456
  }, 1000)
  return {
    nameObj,
    copyNameObj
  }
}
```

## toRefs
> 将 proxy 对象 `proxy({ name: 'tang', age: 12 }) 转换成 { name: proxy({ value: 'tang' }), age: proxy({ value: 12 }) }`

> 可以用于解构
```js
setup(props, context) {
  const { reactive, toRefs } = Vue
  const nameObj = reactive({ name: 'tang' })
  setTimeout(() => {
    nameObj.name = '000'
  }, 1000)
  const { name } = toRefs(nameObj)
  return { name }
}
```

## setup
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

## computed 计算属性
```js
  // 计算属性 写法1
  const countAddFive = computed(() => {
    return count.value + 5
  })

  // 2
  const countAddFive = computed({
    // 读取 countAddFive 的时候，同上
    get: () => count.value + 5,
    // 修改 countAddFive 的时候
    set: (param) => count.value = param - 10
  })

  // 测试 set 方法
  setTimeout(() => {
    countAddFive.value = 100
  }, 1000);
```

## watch 侦听器
```js
// watch 具备一定的惰性 lazy，首次不会执行

setup() {
  const { ref, reactive, watch, watchEffect, toRefs } = Vue

  // ref
  const name= ref('')
  watch(name, (curVal, preVal) => {})

  // reactive
  // 单个属性
  // 接受箭头函数
  const nameObj1= reactive({ name: 'tang' })
  watch(() => nameObj.name, (curVal, preVal) => {})

  // 多个属性
  // 函数 数组
  const nameObj2= reactive({ name: 'tang', engName: 'lucifer' })
  watch(
    [() => nameObj.name, () => nameObj.engName], 
    ([curName, curEng], [preName, preEng]) => {
      ...
    }
  )
}
```

## watchEffect
> watch 具备一定的惰性 lazy，首次不会执行，而 watchEffect 没有惰性

```js
setup() {
  const { ref, reactive, watch, watchEffect, toRefs } = Vue
  const count= ref(0)

  // 立即执行传入的一个函数，
  watchEffect(() => {
    console.log(count.value)    // 0
  })
  setTimeout(() => {
    count.value ++              // 1
  }, 1000);

}
```


