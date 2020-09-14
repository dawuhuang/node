# Vue 路由
路由的本质是一种对应关系，比如在url地址中输入要访问的url地址之后，浏览器要去请求url对应的资源，那么url地址和真实的资源之间就要一种对应关系，就是路由。
路由分为前端路由和后端路由
后端路由是由服务器端进行实现，并完成资源的分发
前端路由时依靠hash值（锚链接）的变化实现的。前端路由主要做的事情就是监听事件，并分发事件处理函数。

## 前端路由的体验
前端路由核心依靠的是一个事件
```js
window.onhashchange = function() {
    location.hash
}
```
前端路由实现tab栏切换
```js
    <div id="app">
        <a href="#/zhuye">主页</a>
        <a href="#/keji">科技</a>
        <a href="#/caijing">财经</a>
        <a href="#/yule">娱乐</a>
        <!--  component 组件占位符 :is绑定的是组件名称 -->
        <component :is='comName'></component>
    </div>
    <script>
        const zhuye = {
            template: `<h1>主页信息</h1>`
        }
        const keji = {
            template: `<h1>科技信息</h1>`
        }
        const caijing = {
            template: `<h1>财经信息</h1>`
        }
        const yule = {
            template: `<h1>娱乐信息</h1>`
        }
        const vm = new Vue({
            el: '#app',
            data: {
                comName: 'zhuye'
            },
            components: {
                zhuye,
                keji,
                caijing,
                yule,
            }
        })
        window.onhashchange = function() {
            switch(location.hash.slice(2)) {
                case 'zhuye': 
                    vm.comName = 'zhuye'
                    break
                case 'keji': 
                    vm.comName = 'keji'
                    break
                case 'caijing': 
                    vm.comName = 'caijing'
                    break
                case 'yule': 
                    vm.comName = 'yule'
                    break
            }
        }
    </script>
```
## VueRouter
支持H5历史模式或者hash模式
支持嵌套路由，路由参数，编程式路由，路由的导航守卫，路由过渡动画特效，路由懒加载，滚动行为
### 使用步骤
1. 引入js文件
    ```js
    <script src="./js/vue.js"></script>
    <script src="./js/vue-router_3.0.2.js"></script>
    ```
2. 使用 `<router-link to='/user'>User</router-link>` router-link会被渲染成a标签，to属性会被渲染成href属性
3. 使用占位符 `<router-view></router-view>`,对应的组件会被渲染到这里
4. 生成组件
    ```js
        // 生成组件
    const User = {
        template: `<div>THIs IS USER</div>`
    }
    ```
5. 定义规则，并实例化路由对象
    ```js
    const myRouter = new VueRouter({
        // 定义规则
        routes: [
            // redirect 路由重定向
            {path:'/', redirect:'/user'},
            // path 对应路径  component 对应组件
            {path: '/user', component:User}
        ]
    })
    ```
7. 挂载到实例上
    ```js
    new Vue({
        el: '#app',
        router: myRouter
    })
    ```
## 嵌套路由
当我们想访问路由的子级路由时，就需要嵌套路由。在路由规则中有个 `children`属性，将子路由规则写道里面。
```js
    <div id="app">
        <router-link to='/user'>user</router-link>
        <router-view></router-view>
    </div>
    <script>
        const User = {
            template: `<div>
            <h3>this is uset</h3>
            <br />
            <router-link to='/user/info'>info</router-link>
            <router-view></router-view>

            </div>`
        }
        const Info = {
            template: `<h3>Info</h3>`
        }
        const myRouter = new VueRouter({
            routes: [
                {path: '/user',component: User,children: [
                    {path:'/user/info',component:Info}
                ]}
            ]
        })
        new Vue({
            el: '#app',
            router:myRouter
        })
    </script>
```

## 动态路由
1. 使用$route.params.id 获取路径传参的数据。
```js
const User = {
    template: `<div>用户名{{$route.params.id}}</div>`
}
const myRouter = new VueRouter({
    routes: [
        {path:'/user/:id', component: User}
    ]
})
```
2. props: true
```js
const User = {
    props:['id'],
    template: `<div>用户名{{id}}</div>`
}
const myRouter = new VueRouter({
    routes: [
        //如果设置为 props: true ,route.params将会被数组为组件的属性
        {path:'/user/:id', component: User,props: true}
    ]
})
```
3. 可以将props设置为对象
```js
const User = {
    props:['username','age'],
    template: `<div>用户名{{username}}---- {{age}}</div>`
}
const myRouter = new VueRouter({
    routes: [
        //如果设置为 props: true ,route.params将会被数组为组件的属性
        {path:'/user/:id', component: User,props: {username:'zs',age: 20}}
    ]
})
```
4. 如果想要获取传递的参数值还想要获取传递的对象数据，那么props应该设置为函数形式
```js
const User = {
	props: ["username", "age", "id"],
	template: `<div>用户名{{username}}---- {{age}}---{{id}}</div>`,
};
const myRouter = new VueRouter({
	routes: [
		//如果设置为 props: true ,route.params将会被数组为组件的属性
		{
			path: "/user/:id",
			component: User,
			props: route => {
                return  { username: "zs", age: 20, id: route.params.id }
            },
		},
	],
});
```
## 命名路由
在route规则中通过name ,设置别名
```js
var myRouter = new VueRouter({
    //routes是路由规则数组
    routes: [
        //通过name属性为路由添加一个别名
        { path: "/user/:id", component: User, name:"user"},
        
    ]
})
```
添加了别名之后，可以使用别名进行跳转
```js
    // name 规则的名字  parasm 传递的参数
<router-link :to="{ name:'user' , params: {id:123} }">User</router-link>
```
## 函数式导航
调用js的api方法实现导航
```js
Vue-Router中常见的导航方式：
this.$router.push("hash地址");
this.$router.push("/login");
this.$router.push({ name:'user' , params: {id:123} });
this.$router.push({ path:"/login" });
this.$router.push({ path:"/login",query:{username:"jack"} });
 // 前进或者后退
this.$router.go( n );//n为数字，参考history.go
this.$router.go( -1 );
```