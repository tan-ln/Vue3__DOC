# 插件
> 封装通用功能
1. 
```js
  const myPlugin = {
    // app => vue 根实例，options 传递的参数
    install(app, options) {
      app.provide('name', options.name)

      app.directive('focus', {
        mounted(el) {
          el.focus()
        }
      })

      app.mixin({
        mounted() {
          console.log('mixin')
        }
      })
      // 对全局属性 扩展
      // 属性名 加 $ 表示 私有
      app.config.globalProperties.$sayHello = 'hello world'
    }
  }

  // 使用插件
  app.use(myPlugin, {
    name: 'tang'
  })
```
2. 写法2
```js
  const validatorPlugin = (app, options) => {}
```

