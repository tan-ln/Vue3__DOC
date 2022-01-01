# Vue3

```js
// 创建从 createApp 开始，vue3 的初始化
const app = Vue.createApp({
  data() {
    return {}
  },
  methods: {
    fn() {}
  },
  template: `<div></div>`
})

// vm: vue 应用的根组件
const vm = app.mount('#root')

// vm.$data 全局的 data 对象

```

## 生命周期钩子
`app = Vue.createApp({}).mounted('#root')`

- 应用初始化 vue3

`beforeCreate() {}`

- 初始化 init => 数据观测(data Observer)和 event/watch 事件配置之前调用
- 没有 data 没有 实例

`created() {}`

- 实例创建完毕
- 数据观测、计算属性、方法事件监听的回调函数 已完成
- 可用于异步获取数据


> 判断是否存在 template，
> 是则 把 template 编译进 render 函数，结合 data 数据，渲染
> 没有则  把挂载节点的 HTML 当做 template 编译，compile el's innerHTML as template


`beforeMount() {}`

- 挂载开始前
- 组件内容被渲染到页面之前
- {{}} 中的内容未被编译替换

`mounted() {}`

- 实例挂载 app.$el(与 vue 实例关联的 dom 元素) 替换 el(真实 dom)
- 但不保证所有子组件一起挂载(如果希望所有视图都完成渲染可在内部调用 *wm.$nextTick()*)
- {{}} 占位符中的内容被编译替换
- 组件内容被渲染到页面

`beforeUpdate() {}`

- 数据发生变化 ，页面 未渲染
- 适合现有 dom 将要被更新前 访问，如移除 *手动添加的事件监听器*

`updated() {}`

- 虚拟 dom 重新渲染，页面更新
- 避免此时的状态更改，最好使用 计算属性或侦听器
- 同 mounted 方法，不保证所有更新完成，可 *wm.$nextTick()*

`beforeUnmount() {}`

- vue 不再接管 el，并销毁实例 $el
- 此时 实例 未失效

`unmounted() {}`

- 实例已失效，解除绑定，移除监听


## 生命周期函数
```js
setUp() {

}
```

## 指令
1. v-on:click = @click

*阻止默认事件*：`@click.prevent=""`

2. v-bind
> v-bind: sth
```js
data() {
  return {
    disable: false
  }
},
template: `<input :disabel="disable" />`
```

> v-bind="params" 多个参数放进一个对象内
```js
data() {
  return {
    params: {
      a: 'a',
      b: 'b',
      c: 'c',
      d: 'd'
    }
  }
}

  // <div v-bind="parmas"></div> === <div :a="a" :b="b" ... ></div>

props: ['a', 'b', 'c', 'd']

```

3. v-model
4. v-for
5. v-html
```js
data() {
  return {
    conetnt: '<strong>asd</strong>'
  }
},
template: `<div v-html="content"><div>`
```
6. v-once 只显示一次
7. v-if  v-else/ v-else-if
8. v-show

动态属性：
```js
data() {
  return {
    message,
    name: 'title',
    event: 'click'
  }
},
methods: {
  handleClick() {}
},
template: `
  <div
    @[event]="handleClick"
    :[name]="message"
  >
    {{message}}
  </div>
`
```


## data && methods && computed && watch
1. `vm.$data.sth` 或者在根 data 直接 `vm.sth`
2. methods
箭头函数找不到 this
3. 区别
`computed` 
    计算属性 是有缓存的 ，
    只有*依赖的内容发生改变*时，才会重新计算，   更像*属性*一样访问，
`methods` 
    只要*页面重新渲染*，就会重新计算，           像*方法*一样调用

4. watch
- 属性监听，可以获取 变化后的值和之前的值
- 异步操作可以使用 watch
- 同步 能用 computed 尽量用 computed
```js
created() {
  // 只能将 顶层 数据作为字符串传递
  this.$watch('attrName', (newVal, oldVal) => {})

  // 单个嵌套 属性的 用函数返回。 data() { return { a: { b: 1} } }
  this.$watch(() => this.a.b, (newVal, oldVal) => {})

  // 复杂表达式
  this.$watch(() => this.a + this.b, (newVal, oldVal) => {})
}
```
- 对于监听的值是一个 **对象或者数组** 时，他内部属性或者元素的更改不会触发监听器，*引用类型*
- `deep: true` 选项可以深度监听 对象或数组内部的变化，要注意此时 (newVal = oldVal)，因为引用指向一个值

```js
  this.$watch('obj', callback, {
    deep: true
  })
```


## 修饰符
1. 事件修饰符
`@click.prevent`    阻止默认行为
`@click.stop`       阻止冒泡
`@click.self`       元素本身触发，子元素无效
```html
<div @click.self="handleDivClick">
  {{sth}}
  <button @click="handleBtnClick">button</button>
</div>
```
`@click.capture`    使用事件捕捉模式
`@click.once`       只触发一次

2. 按键修饰符
`@keydown.enter | tab | delete | esc | left | down ...`

3. 鼠标修饰符
`@click.left | right | middle`     鼠标左(右 | 中)键单击

4. 精确修饰符 exact
`@click.ctrl.exact`

5. `lazy` 不立即操作

6. `number` 类型转换

7. `trim` 去除前后空格

## v-model
0. input | textarea
1. checkbox
```js
data() {
  return {
    checkBoxVal: []      // 数组收纳多个 CheckBox 值
  }
},
template: `
  <div>
    {{checkBoxVal}}
    <input type="checkbox" v-model="checkBoxVal" value="aaa" />
    <input type="checkbox" v-model="checkBoxVal" value="bbb" />
    <input type="checkbox" v-model="checkBoxVal" value="ccc" />
  </div>
`
```

2. radio
```js
data() {
  return {
    radioVal: ''      // 字符串存储 单选按钮 值
  }
},
template: `
  <div>
    {{radioVal}}
    <input type="radio" v-model="radioVal" value="aaa" />
    <input type="radio" v-model="radioVal" value="bbb" />
    <input type="radio" v-model="radioVal" value="ccc" />
  </div>
`
```

3. select
```js
data() {
  return {
    selectVal: [],
    options: [
      {
        text: 'A', value: 'A'
      },
      {
        text: 'B', value: 'B'
      },
      {
        text: 'C', value: 'C'
      },
    ]
  }
},
template: `
  <div>
    {{selectVal}}
    <select v-model="selectVal" multiple>
      <option v-for="item in options" :value="item.value">{{item.text}}</option>
    </select>
  </div>
`
```

## component
1. 全局组件
```js
const app = Vue.createApp({
  template: `
    <div>
      <component-name />
    </div>
  `
})

app.component('component-name', {})
```

2. 局部组件
```js
const Child = {
  data() {}
}
const app = Vue.createApp({
  props: [ Child ],
  template: `
    <div>
      <component-name />
    </div>
  `
})
```

## props
1. 
```js
props: [ Content ]
```
2. 
```js
app.component('test', {
  props: {
    content: {
      type: Number,
      required: true,
      default: 123,
      validator: function(value) {
        return value < 1000
      },
    }
  }
})
```

## Non-props  
> 父组件向子组件传值，子组件不接收
> 会把传递过来的值直接显示在子组件属性中

```js
app.component('child', {
  inheritAttrs: false       // 不接收，不显示
})
```

## $attrs
> 子组件多个根节点，接收传值的问题
```js
const app = Vue.createApp({
  template: `
    <div>
      <child msg="message" a="a" b="b" />
    </div>
  `
})

app.component('child', {
  template: `
    <div :msg="$attrs.msg"></div>
    <div v-bind="$attrs"></div>
    <div></div>
  `
})
```

## 自定义属性 && $options
> `$options` 可以获取 vue 实例上的属性 *data | methods | mixins | render | template）*

```js
const app = Vue.createApp({
  // 自定义属性
  number: 100
})
```

## 父组件传值
1. `this.$emit` 方法

2. *emits 参数*

```js
handleAdd(count) {
  this.count += 1
}

<div>
  <child @add="handleAdd" />
</div>

app.component('child', {
  props: [],
  // emits: ['add'],
  emits: {
    add: (count) => {
      return count > 0 ? true : false     // 校验
    }
  },
  methods: {
    handleClick: function() {
      this.$emit('add', count)
    }
  }
})
```

3. *modelValue*

```js
const app = Vue.createApp({
  data() {
    return {
      count: 1
    }
  },
  template: `
    <counter v-model="count" />
  `
})

app.component('counter', {
  props: ['modelValue'],
  methods: {
    handleClick: function() {
      this.$emit('update:modelValue', this.modelValue + 3)
    }
  },
  template: `
    <div @click="handleClick">{{modelValue}}</div>
  `
})

```

## modelValue(vue3)
> vue2 中 v-model 的写法

`<ChildComponent v-model="title" />` === `<ChildComponent :value="title" @input="title = $event" />`

> 实质是传递一个属性 value，接受一个 input 事件

*input 和 value 不一定适合所有元素的值和事件触发方法*

> vue2.2 的解决方法
```js
export default {
  model: {
    prop: 'title',    // v-model 绑定的属性名
    event: 'change'   // v-model 绑定的事件
  },
  props: {
    value: String,
    title: {          // title 是跟 v-model 绑定的属性
      type: String,
      default: 'default'
    }
  }
}
```

> vue 3
*v-model 用 modelValue 代替 value，update:modelValue 代替 input*
`<ChildComponent v-model="title" />` === `<ChildComponent :modelValue="title" @update="modelValue = $event" />`

1. 
```js
props: {
  modelValue: String,
}

this.$emit('update:modelValue', params)
```

2. 
```js
<ChildComponent v-model:title="title" />

props: {
  title: String
}

this.$emit('update:title', params)
```

3. 
```js
<ChildComponent v-model.uppercase="title" />      // 加自定义修饰符 大写 

props: {
  'modelValue': {
    type: String
  },
  'modelModifiers': {       // 接收修饰符
    default: () => ({})
  }
},
methods: {
  handleClick() {
    let newVal = this.modelValue + 'a'
    if(this.modelModifiers.uppercase) {
      ...
    }
  }
}

```

## slot 插槽
> 父组件向子组件传递 dom 节点、字符串、子节点，props 传递不便
> 将会替换掉子组件的 slot，
> slot 标签本身无法绑定 点击事件
> <slot>default value</slot> 未传递则使用默认

```js
  const app = Vue.createApp({
    template: `
      <myform>
        <div>提交</div>
      </myform>
      <myform>
        <button>提交</button>
      </myform>
    `
  })

  app.component('myform', {
    methods: {
      handleClick() {
        alert('123')
      }
    },
    template: `
      <div>
        <input />
        <span @click="handleClick">
          <slot>default value</slot>
        </span>
      </div>
    `
  })
```

- `v-slot:` 具名插槽
```js
<layout>
  <template v-slot:header>        占位符 template 不会显示任何标签
    <div>header</div>
  </template>
  <template v-slot:footer>        可简写为 #header | #footer
    <div>footer</div>
  </template>
</layout>

<div>
  <slot name="header"></slot>
  <div>content</div>
  <slot name="footer"></slot>
</div>
```

- 作用域插槽
```js
// app
<list v-slot="slotProps">         简写使用解构：v-slot="{item}"
  <div>{{slotPorps.item}}</div>
</list>

data() {
  return {
    list: [1, 2, 3]
  }
}

// component list
<div>
  <slot v-for="item in list" :item="item" />   传递给父组件 slotPorps 对象
</div>
```

## 动态组件 `<component :is="currentItem" />`

```js
data() {
  return {
    currentItem: 'input-item'
  }
},
methods: {
  handleClick() {
    this.currentItem === 'input-item' ? 'common-item' : 'input-item'
  }
},
// keep-alive 有缓存，通常配合动态组件使用
template: `
  <keep-alive>
    <component :is="currentItem" />
  </keep-alive>
  <button @click="handleClick">切换</button>
`

// input-item component
app.component('input-item', {})

// common-item component
app.component('common-item', {})
```

## 异步组件 `Vue.defineAsyncComponent`
```js
app.component('async-common-item', Vue.defineAsyncComponent(() => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve({
        template: `<div>异步组件</div>`
      })
    }, 2000)
  })
}))
```

## Ref
> 获取 dom 节点或者 组件 的引用

```js
mounted() {
  this.$refs.count  // <div>123</div>
}

template: `
  <div>
    <div ref="count">123</div>
  </div>
`
```

> 获取组件 的方法
> this.$refs.common.getName()
```js
<common ref="common" />

// common component
methods: {
  getName() {
    console.log('name')
  }
}
```


## provide / inject 多层组件传值
```js
// app component
Vue.createApp({
  provide: {
    count: 1
  }
})

// component child-child 孙子组件
app.component('child-child', {
  inject: ['count']
})
```

> data 中的属性*不能*直接传递 `provide: { count: this.count }`
> 将 provide 转为 函数
> 这种方法是*一次性*的，count 值的改变不会影响传递过去的
```js
provide() {
  return {
    count: this.count
  }
}
```
