### Vue Router
* [看看文档吧](https://router.vuejs.org/zh/guide/#html)
* 要想实现单页面应用，就得用上Vue + Vue-Router
* 就是嘛，将组件映射到路由（routes），然后告诉Vue Router在哪里渲染他们

#### 使用
* 直接页面级的开发，script直接引入Vue
* 工程性开发（模块化开发），嗯...项目中安装第三方模块(使用 npm/cnpm/yarn 安装)，我一般用 npm install vue-router。

#### Api
##### ```<router-link></router-link>```
* Props
    * to
        * type: string | Location
        * required
        * 表示目标路由的链接。当被点击后，内部会立刻把 to 的值传到 router.push()，所以这个值可以是一个字符串或者是描述目标位置的对象。
        ``` vue
            <router-link to="home">Home</router-link>
            <router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
            <router-link :to="{ path: 'register', query: { plan: 'private' }}">Register</router-link>
        ```       
    * replace
        * type: boolean
        * default: false
        * 当点击时，会调用 router.replace() 而不是 router.push()，于是导航后不会留下 history 记录。
        ``` vue
            <router-link :to="{ path: '/abc'}" replace></router-link>
        ```
    * append
        * type: boolean
        * defuat: false
        * 设置 append 属性后，则在当前 (相对) 路径前添加基路径。
        ```vue
            <router-link :to="{ path: 'relative/path'}" append></router-link>
        ```
    * tag
        * type: string
        * default: "a"
        * 使用此属性，可以将router-link渲染成指定标签，当然，点击也会触发导航
        ``` vue
            <router-link to="/foo" tag="li">foo</router-link>
            <!-- 渲染结果 -->
            <li>foo</li>
        ```
    * active-class
        * type: string
        * default: "router-link-active"
        * 嗯...简单说就是链接被激活是使用的class类名，默认值可以通过路由的构造选项 linkActiveClass 来全局配置（两种方式）
        ``` js
            // 直接在配置路由的js文件配置linkActiveClass
            export default new Router({
                linkActiveClass: 'active',
                ...
            })
        ```
        ``` vue
            // 在router-link中写入active-class
            <router-link to="/home" class="menu-home" active-class="active">首页</router-link>
        ```
    * exact
        * type: boolean
        * default: false
        * 嗯...上面阐述了如何设置类名，那就出现一个问题，'/','/my'，这两个路径对应的类名都会被匹配到，所以说只要是匹配到'/'的路由都会激活默认类名，exact就是严格匹配到'/my'的时候才激活类名
        ``` vue
            <!-- 这个链接只会在地址为 /my 的时候被激活 -->
            <router-link to="/my" exact>
        ```
    * event
        * type: string | Array<string>
        * default: "click"
        * 声明可以用来触发导航的事件。可以是一个字符串或者是数组字符串

    * exact-active-class
        * type: string
        * default: 'router-link-exact-active'
        * 配置当链接被精确匹配的时候应该激活的 class。注意默认值也是可以通过路由构造函数选项 linkExactActiveClass 进行全局配置的。
    
##### ```<router-view></router-view>```
    <router-view> 组件是一个 functional 组件，渲染路径匹配到的视图组件。<router-view> 渲染的组件还可以内嵌自己的 <router-view>，根据嵌套路径，渲染嵌套组件。    
* Props
    * name
        * type: string
        * default: "default"  
        * 如果router-view配置了name属性，则会匹配到路由配置中components下相的组件
##### Router构建选项
* routes
    * type: Array<RouteConfig>
    * RouteConfig的定义类型
    ``` js
        const routes = [
            { path: '/foo', component: Foo , name: 'foo'},
            { path: '/bar', component: Bar , name: 'bar'}
        ]

        const router = new VueRouter({
            routes 
        })
    ```
* mode
    * type: string
    * default: "hash" (浏览器环境) | "abstract"(Node.js 环境)
    * options: "hash" | "history" | "abstract"
        ```
        hash: 使用 URL hash 值来作路由。支持所有浏览器，包括不支持 HTML5 History Api 的浏览器。

        history: 依赖 HTML5 History API 和服务器配置。查看 HTML5 History 模式。

        abstract: 支持所有 JavaScript 运行环境，如 Node.js 服务器端。如果发现没有浏览器的 API，路由会自动强制进入这个模式。
        ```
* base
    * type: string
    * default: "/"
    * 应用的基路径。例如，如果整个单页应用服务在 /app/ 下，然后 base 就应该设为 "/app/"
* linkActiveClass
    * type: string
    * default: "router-link-active"
    * 全局配置```<router-link>```的默认"激活class类名"
* linkExactActiveClass
    * type: string
    * default: "router-link-exact-active"
    * 全局配置```<router-link>```精确激活默认的class类
* scrollBehavior
    * type: function
    ``` js
        const router = new VueRouter({
            routes: [...],
            scrollBehavior (to, from, savedPosition) {
                // return 期望滚动到哪个的位置
                //scrollBehavior 方法接收 to 和 from 路由对象。第三个参数 savedPosition 当且仅当 popstate 导航 (通过浏览器的 前进/后退 按钮触发) 时才可用。

                //异步
                return new Promise((resolve, reject) => {
                    setTimeout(() => {
                    resolve({ x: 0, y: 0 })
                    }, 500)
                })
        }
})
    ```
