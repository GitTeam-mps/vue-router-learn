## vue-router 1.0学习demo

## 目录

* [demo1基本demo](## demo1基本demo)



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
[demo2 链接](https://gitteam-mps.github.io/vue-router-learn/html/demo2.html "嵌套路由demo2")


> ## demo3 具名路由

```javascript
   @具名路由
   <li v-link="{name:'detail',params:{id:'01'} }">News 01</li>
   就是用名字来确定那个路由组件

  <ul>
        <li v-link="{name:'detail',params:{id:'01'} }">News 01</li>
        <li v-link="{path:'/home/news/detail02'}">News 02</li>
        <li v-link="{path:'/home/news/detail03'}">News 03</li>
  </ul>
  router.map({
    '/home':{
        component:Home,
        subRoutes:{
            '/news':{
                component:News,
                subRoutes:{
                    '/detail01':{
                        name:'detail',
                        component:Detail01,
                    },
                    '/detail02':{
                        component:Detail02,
                    },
                    '/detail03':{
                        component:Detail03,
                    },
                },
            },
            '/message':{
                component:Message
            },
        },
    },
    '/about':{component:About}
});
  
 在子路由 可以通过名字 把长的路由变得更加容易识别。
```
[demo3 链接](https://gitteam-mps.github.io/vue-router-learn/html/demo3.html "具名路由 demo3")


> ## demo4 选中样式和父级选中样式的办法

```javascript
   一般的需要选中的时候给默认的class就可以
    <a v-link="{path:'/home',activeClass: 'active'}">home</a>
    <a v-link="{path:'/about',activeClass: 'active'}">About</a>
  选中会给选中项添加active 这个class
  
  但是有时候我们的需求是在 `*a*` 的 父元素 上添加类型 该怎么处理（学习网上的方法）
    <li :class="currentPath == '/home/news' ? 'active2': ''">
       <a v-link="{ path: '/home/news'}">news</a>
   </li>
   <li :class="currentPath == '/home/message' ? 'active2': ''">
       <a v-link="{ path: '/home/message'}">message</a>
   </li>
  
  var Home=Vue.extend({
    template:'#home',
    data: function() {
        return {
            currentPath: ''
        }
    },
     route: {
         data: function(transition){
             transition.next({
                 currentPath: transition.to.path
             })
         }
     }
 });
  在这个组件的钩子函数的data期间 定义一个变量 然后把路径的名字付给变量 ，在上面判断是不是应该显示
  
```
[demo4 链接](https://gitteam-mps.github.io/vue-router-learn/html/demo4.html "选中样式处理 demo4")


> ## demo5 路由的全局函数钩子和组件钩子以及每个钩子的对象

路由的切换过程，本质上是执行一系列路由钩子函数，钩子函数总体上分为两大类：
    全局的钩子函数
    组件的钩子函数
全局钩子函数
    全局钩子函数有2个：
    beforeEach：在路由切换开始时调用
    afterEach：在每次路由切换成功进入激活阶段时被调用
组件的钩子函数
    组件的钩子函数一共6个：
    data：可以设置组件的data
    activate：激活组件
    deactivate：禁用组件
    canActivate：组件是否可以被激活
    canDeactivate：组件是否可以被禁用
    canReuse：组件是否可以被重用
切换对象
每个切换钩子函数都会接受一个 transition 对象作为参数。这个切换对象包含以下函数和方法
    transition.to
    表示将要切换到的路径的路由对象。
    transition.from
    代表当前路径的路由对象。
    transition.next()
    调用此函数处理切换过程的下一步。
    transition.abort([reason])
    调用此函数来终止或者拒绝此次切换。
    transition.redirect(path)
    取消当前切换并重定向到另一个路由。


```javascript
function RouteHelper(name) {
    var route = {
        canReuse: function(transition) {
            well.setColoredMessage('执行组件' + name + '的钩子函数:canReuse')
            return true
        },
        canActivate: function(transition) {
            well.setColoredMessage('执行组件' + name + '的钩子函数:canActivate')
            transition.next()
        },
        activate: function(transition) {
            well.setColoredMessage('执行组件' + name + '的钩子函数:activate')
            transition.next()
        },
        canDeactivate: function(transition) {
            well.setColoredMessage('执行组件' + name + '的钩子函数:canDeactivate')
            transition.next()
        },
        deactivate: function(transition) {
            well.setColoredMessage('执行组件' + name + '的钩子函数:deactivate')
            transition.next()
        },
        data: function(transition) {
            well.setColoredMessage('执行组件' + name + '的钩子函数:data')
            transition.next()
        }
    }
    return route;
}

var Home=Vue.extend({
    template:'#home',
    data: function() {
        return {
            currentPath: ''
        }
    },
    route:  RouteHelper('Home'),

});
通过这样的方式 每个组件经理的函数钩子过程都会被记录下来
```
[demo5 链接](https://gitteam-mps.github.io/vue-router-learn/html/demo5.html "函数钩子 demo5")

> ## demo5 路由的全局函数钩子和组件钩子以及每个钩子的对象


 切换控制流水线
    执行router的全局函数:beforeEach

    执行组件Home的钩子函数:canActivate

    执行组件Message的钩子函数:canActivate

    执行router的全局函数:afterEach

    执行组件Home的钩子函数:activate

    执行组件Home的钩子函数:data

    执行组件Message的钩子函数:activate

    执行组件Message的钩子函数:data
    
1. 可重用阶段

  检查当前的视图结构中是否存在可以重用的组件。这是通过对比两个新的组件树，找出共用的组件，然后检查它们的可重用性（通过 canReuse 选项）。
默认情况下， 所有组件都是可重用的，除非是定制过。

2. 验证阶段
检查当前的组件是否能够停用以及新组件是否可以被激活。这是通过调用路由配置阶段的canDeactivate 和canActivate 钩子函数来判断的。

3.激活阶段

一旦所有的验证钩子函数都被调用而且没有终止切换，切换就可以认定是合法的。
路由器则开始禁用当前组件并启用新组件。
此阶段对应钩子函数的调用顺序和验证阶段相同，其目的是在组件切换真正执行之前提供一个进行清理和准备的机会。
界面的更新会等到所有受影响组件的 deactivate 和 activate 钩子函数执行之后才进行。
data 这个钩子函数会在 activate 之后被调用，或者当前组件组件可以重用时也会被调用。

[demo6 链接](https://gitteam-mps.github.io/vue-router-learn/html/demo6.html "切换过程 demo6")
    
