# mixin 混入
>  *data | methods | 自定义属性* ,优先使用*组件内部*，其次选择 *mixin* data
> *生命周期函数*，先执行 *mixin* 的，再执行 *组件内*的
> *局部 mixin*只能应用于 *声明调用的 组件内*，子组件无效（若要使用则子组件再调用）

```js
  const myMixin = {
    data() {
      return {
        number: 2
      }
    },
    created() {},
    methods: {
      fn() {}
    }
  }

  const app = Vue.createApp({
    // 自定义属性
    number: 100,
    // 声明调用 mixin
    mixins: [myMixin],
  })
``` 

## 全局 mixin
> 其他组件无需声明注入，自动调用
```js
app.mixin({
  data() {},
  methods: {}
})
```

## 属性策略修改 *组件内属性和 mixin 优先级*
```js
app.config.optionMergeStrategies.number = (mixinVal, appVal) => {
  return mixinVal || appVal
}
```


