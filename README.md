## vue-router 1.0学习demo

> ## demo1基本demo

```javascript
   * 定义组件
      var Home=Vue.extend({
          template:'<div>my name is hello</div>',
      });
      
   * 创建 router 实例，然后传 routes 配置
      var router=new VueRouter();
      
   * 映射路由
      router.map({
          '/home':{component:Home},
          '/about':{component:About}
      });
    
  
   
   * 设置首次进入的默认路由 （可不要）
       router.redirect({
            '/': '/home'
        })
        
   *  使用v-link指令
      <a v-link="{path:'/home'}">home</a>
      <a v-link="{path:'/about'}">About</a>
   
   * 使用<router-view>标签
      <router-view></router-view>
     
   * 创建和挂载根实例。 启动路由
     var App = Vue.extend({})
     router.start(App, '#app')
     
```

[demo1 链接](https://gitteam-mps.github.io/vue-router-learn/html/demo1.html "基础demo1")

> ##  demo2 嵌套路由

``` javascript
* @   Vue-router嵌套路由
* @  子路由 要写对 subRoutes
* @   给路由添加子路由 默认可以 这么添加

    router.redirect({
        '/home': '/home/message'
    })

* 
  router.map({
      '/home':{
          component:Home,
          subRoutes:{
              '/news':{
                  component:News
              },
              '/message':{
                  component:Message
              },
          },
      },
      '/about':{component:About}
  });
```
[demo1 链接](https://gitteam-mps.github.io/vue-router-learn/html/demo2.html "嵌套路由demo2")












