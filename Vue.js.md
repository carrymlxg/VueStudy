#Vue.js

代码地址：github.com/hemiahwu/vue-basic

双向数据绑定

组件化开发

单页面应用

## 引入

`<script></script>`标签对

cdn服务器

NPM

## 数据绑定以及指令

```HTML
<div id="app">{{msg}}</div><!--hello world!--> 
```

```javascript
new Vue({
 	el:'#app',
    data:{
       msg:'hello world!' ,
        age:30,
    },
    methods:{
    	getage:function(){
    		return this.age
        }
	}
})
```
**注意：如果在`template`中定义了内容，那么优先加载`template`**

### 指令语法

v-指令名字+:+指令的参数 = 指令的表达式

### v-bind

```html
<div id="app">
    <a v-bind:href='url'>百度</a>
</div>
<script>
    new Vue({
        el:"#app",
        data:{
            url:'http://www.baidu.com'
        }
    })
</script>
```

###双向数据绑定v-model = ""

```html
<div id='app'>
	<input type="text" v-model='value'>
</div>
<script>
    new Vue({
        el:"#app",
        data:{
            value:"abc"
        }
    })
</script>
```

### 修饰符

`v-model.trim`//去空

`v-model.number`//数字类型转换

`v-model.lazy`//失去焦点才会改变

### 输出dom节点 v-html=""

```html
<div id = 'app'>
	{{html}}<--<h1>hello</h1>-->
    <div v-html = "html"></div><--hello-->
</div>
<script>
    new Vue({
        el:'#app',
        data:{
            html:'<h1>hello</h1>'
        }
    })
</script>
```

### 仅渲染一次v-once

### 条件渲染v-if

```html
<div id = 'app'>
    <p v-if='value<100'>不多了</p>
    <p v-else-if="value>=200">
        快来买
    </p>
</div>
<script>
    new Vue({
        el:"#app",
        data:{
            value:200
        }
    })
</script>
```

//v-if 会干掉所有的不显示的dom节点，即页面不加载

###设置元素`display:none`v-show

### `v-if` 与 `v-show`
`v-if`是真正的条件渲染，因为它会确保在切换过程中条件内事件监听器和子组件适当的被销毁和重建
`v-if`也是惰性的：如果最初的渲染条件为假的话，则什么也不做，--直到条件第一次变为真的时候才会渲染
`v-show`不管初始条件是什么，元素总会到被渲染，并且只是单纯的基于css进行切换
一般来说，`v-if`拥有更高的切换开销，而`v-show`有更高的初始渲染开销，因此如果频繁的切换，则使用`v-show`,如果运行是条件很少改变，则使用`v-if`

### VUE未加载时的属性，v-clock

```html
<style>
    div[v-clock]{
        display:none;
    }
</style>
<div id = 'app'>
    <div v-clock>
    	{{msg}}
    </div>
</div>
<script>
    setTimeOut(function(){
      new Vue({
        el;"#app",
        data:{
    		msg:'hello'    
	    }
    })   
    },5000)
    
</script>
```

### 列表渲染v-for

```html
<div id="app">
    <h1>电影列表</h1>
    <ul>
    <li v-for="(t,i) in movie">{{t}}</li>
</ul>
</div>
<script >
    new Vue({
        el:"#app",
        data:{
            movie:[
                {title:'我不是药神',yanyuan:'徐峥'},
                {title:'碟中谍',yanyuan:'汤姆克鲁斯'}
            ]
        }
    })
</script>

```

### Vue.set

```html
    <div id="app">
        <ul>
            <li v-for="n in number">{{n}}</li>
        </ul>
        <botton v-on:click="changeNumber">click me</botton>
    </div>
    <script >
        new Vue({
            el:'#app',
            data:{
                number:[1,2,3,4,5]
            },
            methods:{
                changeNumber:function() {
                  this.number[1] = 10;
                  alert(this.number[1])
                }
            }
        })
    
</script>
```
我们通过点击事件想改变页面中的值，但是发现无法改变，这是因为数组为引用值，索引不会发生改变，那么我们想改变就需要用到Vue.set

```html
    <div id="app">
            <ul>
                <li v-for="n in number">{{n}}</li>
            </ul>
            <botton v-on:click="changeNumber">click me</botton>
        </div>
        <script >
            new Vue({
                el:'#app',
                data:{
                    number:[1,2,3,4,5]
                },
                methods:{
                    changeNumber:function() {
                      Vue.set(this.number,1,10)
                      alert(this.number[1])
                    }
                }
            })
    </script>
```
需要注意的是，v-for会造成模板复用，

```html
<div id="app">
    <div v-for="item in persons">{item.name}
        <input type="text">
    </div>
    <button v-on:click="reverse">click me</button>
</div>
<script>
    new Vue({
        el:"#app",
        data:{
            persons:[
                {id:1,name:'zhangsan'},
                {id:2,name:'lisi'},
                {id:3,name:'wangwu'}
            ]            
        },
        methods:{
            reverse:function() {
              this.persons.reverse();
            }
        }
    })
</script>
```
在input框输入相应的值，点击按钮我们会发现div的内容发生倒置，但是input框却没有，如果我们想让input的内容也跟着变化
我们需要该每一个input绑定一个key值

```html
<div id="app">
    <div v-for="item in persons" v-bind:key='item.id'>{item.name}
        <input type="text">
    </div>
    <button v-on:click="reverse">click me</button>
</div> 
```

## 组件

### 父子组件通信prop

```html
Vue.component('Parent',{
template:`<div>
    <p>父组件</p>
    <Child title='haha'></Child>
</div>`
});
Vue.component('Child',{
template： `<div>
    <p>子组件</p>
    <p>{{title}}</p>
    </p>
</div>`，
props:['title]
})
```

### 子组件向父组件传值this.$emit

接受两个参数，事件名，数据

### 插槽





##计算属性以及watch监听

### 计算属性computed

依赖的数据发生改变才会执行
再次运行相同属性会走缓存

```html
<div id="app">
    {{msg}}
    <br>
    <input type="text" v-model="num">
    <button v-on:clcik="fullname">
        click
    </button>
</div>
<script>
    new Vue({
        el:"#app",
        data:{
            num:111,
            msg:'hello world!',
            name:"zhangsan"
        },
        computed:{
            fullname:function () {
                alert(1)
                return this.msg+this.name
            }
        }
    })
</script>
```
###watch
```html
<div id="app">
    <ul>
        <li v-for="item in fullMovies">{{item}}</li>
    </ul>
    <button v-on:click="addMovie">click</button>
</div>
<script>
    new Vue({
        el:"#app",
        data:{
            movies:[
                {name:'我不是药神',year:2018},
                {name:'捉妖记2',year:2018},
                {name:'西虹市首富',year:2018}
            ]
        },
        computed:{
            fullMovies:function () {
                return this.movies.map(function (movie) {
                    return movie.name + '('+ movie.year  +')';
                })
            }
        },
        methods:{
            addMovie:function () {
               this.movies.push({name:'碟中谍6',year:2018})
            }
        },
        watch:{
            movies:function (newValue) {
                alert("我添加了"+newValue[newValue.length-1].name)
            }
        }
    })
</script>
```
watch 监听的是单个属性

- 基本的数据类型
- 复杂的数据类型 深度监视





##filter过滤选择器及样式变换

```html
<div id="app">
{{msg | upperCase}}
</div>
<script >
    new Vue({
        el:"#app",
        data:{
            msg:'hello world!'
        },
        filters:{
            upperCase:function(val) {
              return val.toString().toUpperCase();
            }
        }      
    })
</script>

```



## 生命周期

`beforeCreate` 

`create` 应用：发送ajax请求

`beforeMount` 挂载数据到dom之前

`mounted` 挂载完成后

`beforeUpdate` 更新dom之前

`updated` 更新dom后

`beforeDestroy`

`destroyed`

内置组件 

`keep-alive`组件切换过程中简组件转台保留在内存中，防止重复渲染



## 组件通信

1. #### props 和 $emit

   父组件向子组件传值用props,子组件向父组件传值用$emit

2. ####$attrs 和 $listeners 

    $attrs 包含了父作用域中不作为prop被识别的特定属性，可以通过`v-bind=$attrs`

    

3. ####中央事件总线

   new bus = new Vue()
   通过`$on`绑定

4. ####父组件provider，子组件 inject

   - provider | Object | () => Object
   - inject | Array<string> 

   **`provider` 和 `inject 主要为组件库用例，不推荐直接用于应用程序代码`**

   需要一起使用，

5. ####  $parent 和 $children



**$props、$attrs和$listeners**

