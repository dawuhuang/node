# 常用特性
## 1 表单的基本操作
表单中的 `<input>` 基本都有设置 value 属性。复选框的v-model的值要设置为数组，
## 2 表单修饰符
1. number 转换为数值
2. trim 去除两端的空格。
3. lazy 将 input 事件转换为 change 事件。input是实时触发，change事件是失去焦点时触发。
## 3 自定义指令
1. 全局自定义指令
bind只渲染一次
```js
<div id="app">
    <input type="text" v-focus>
    <div v-color='color'>2222</div>
</div>
<script src="./js/vue.js"></script>
<script>
    Vue.directive('focus',{
        inserted: function(el) {
            el.focus()
        }
    })
    // 带参数的自定义指令
    Vue.directive('color', {
        bind: function(el,binding) {
            el.style.color = binding.value
        }
    })
    new Vue({
        el: '#app',
        data: {
            color: 'red'
        }
    })
```
2. 局部自定义指令 directives
```js
directives: {
    focus: {
        inserted: function(el) {
            el.focus()
        }
    },
    color: {
        bind: function(el,binding) {
            el.style.color = binding.value
        } 
    }
}
```
## 4 计算属性computed
计算属性时根据它的依赖来进行缓存的。不加括号，直接调用。计算属性的属性不能时data中已经有的属性
## 5 侦听器 watch
watch用来监听数据的变化，一般用来异步操作和开销较大的操作。watch的属性，必须时data中已经有的属性。
## 6 过滤器
过滤器写在管道符的后面，可以进行串联操作。
过滤器不改变真正的data，而只是改变渲染的结果。
1. 全局过滤器filter
```js
<div id="app">
	<div>{{msg | UP}}</div>
</div>
<script src="./js/vue.js"></script>
<script>
	Vue.filter("UP", function (val) {
		return val.charAt[0].toUpperCase() + val.slice(1);
	});
	var vm = new Vue({
		el: "#app",
		data: {
			msg: "nihao",
		},
	});
```
2. 局部过滤器filters
```js
var vm = new Vue({
	el: "#app",
	data: {
		msg: "nihao",
    },
    filters: {
        UP: function(val) {
            return val.charAt(0).toUpperCase() + val.slice(1);
        }
    }
});
```
3. 带参数的过滤器
```js
//  <div>{{message | filterA(1,2)}}</div>
---------------------------------------
// 带参数的过滤器， 第一个参数时管道符前面的字符
// 第二个参数是第一个实际参数
// 第三个参数是第二个实际参数
Vue.filter('filterA',function(n,a,b) {
    if (n>a) {
        return n + a
    }else {
        return n + b
    }
})
```
## 7 生命周期
vue 常用的声明周期有 8 个
1. 挂载
    1. beforecreat  在实例创建完成之后，事件观测和数据配置之前被调用。
    2. created      在实例创建完成后被立即调用。
    3. beforemount  在实例挂载之前被立即调用。
    4. mounted      el 被新创建的 vm.$el 所代替，在挂载到实例上去之后立即调用钩子。

2. 更新
    5. beforeUpdate 数据更新时调用，发生在虚拟 DOM 打补丁之前
    6. updated      由于数据更新导致虚拟 DOM 重新渲染和打补丁，在这之后调用该钩子。
3. 销毁
    7. beforeDestroy在数据销毁之前调用
    8. destroyed    在虚拟销毁之后调用

## 8 数值变异方法和替换数组
1. 变异方法
变异数组是在保存原有数组不变的请求下，对其进行扩展，是响应式的。
在 Vue 中直接修改对象属性的值无法触发`响应式`，当你直接修改对象属性的值，你会发现，数据变了，但是页面内容没有改变。
    push() 在数组的最后面追加一个元素，返回的是数组的长度
    pop()  删除数组的最后一个元素，返回的是被删除的元素
    unshift() 在数组的最前面追加一个元素，返回的是数组的长度
    shift() 删除数组的最见面的一个元素，返回的是被删除的元素
    splice() 有三个参数，第一个是索引，必写。第二个参数是，要删除的长度，必写，第三个是替换到删除位置的元素。
    sort() 默认将数组按从小到大排序，返回排序后的新数组
    reverse() 将数组翻转，返回翻转后的数组

2. 替换数组
不会改变原数组，但总是返回新数组
    filter  找出符合条件的元素，返回一个新数组
    concat  将几个数组拼接成新的数组，不会影响原数组
    slice   从已有的数组中返回一个指定的元素，不会影响原数组，返回的是一个新数组。
## 9 数组的响应式变化
```js
<div id="app">
    <ul>
        <li v-for='item in list'>{{item}}</li>
    </ul>
    <div>
        <div>{{info.name}}</div>
        <div>{{info.age}}</div>
        <div>{{info.gender}}</div>
    </div>
</div>
<script>
    // Vue.set(vm.items,indexOfitem,newValue)
    // vm.$set(vm.items,indexOfitem,newValue)
    // 参数一表示要处理是数组名称
    // 参数二表示要处理的数组的索引 或者 对象的属性名
    // 参数三表示要处理的数组的值
    // 既可以处理数组，也可以处理对象
    var vm = new Vue({
        el: '#app',
        data: {
            list: ['apple','orange','banban'], 
            info: {
                name: 'zs',
                age: 33
            }
        }
    })
    // vm.list[1]='lemen'    不是响应式
    // vm.$set(vm.list,2,'lemon')   是响应式
    Vue.set(vm.list,1,'lemon')  //  是响应式
    // vm.info.gender = '男'   不是响应式
    vm.$set(vm.info,'gender','111')   //  是响应式
</script>
```

