# vue指令
## 1. 解决闪动问题
代码出现闪烁是因为，在加载的时候先加载HTML,把插值语法当作HTML内容加载到页面上，在加载完js后才把插值语法替换掉，所有看到闪烁。
1. v-cloak 解决绑定数据的闪动,原理是先隐藏，在渲染完成后显示最终的值。
```css
[v-cloak] {
    display: none
}
```
2. <script src='vue.js'></script>  在head头中导入。
3. v-text
## 2. 指令
1. v-html 会解析标签
2. v-text 不会解析标签
3. v-pre 不会进行解析，将内容完整展示出来。
4. v-once 只进行一次渲染。
5. v-bind 属性绑定 可以简写为 `:`
6. v-on 事件绑定 可以简写为@
## 3. 事件修饰符
```html
<!-- stop 阻止事件冒泡 -->
<button @click.stop='handle'>点击</button>
<!-- prevent 阻止事件的默认行为 提交事件事件不再重载页面-->
<form action="" @submit.prevent='handle1'></form> 
<!-- selt 是当前元素自身时触发处理函数 -->
<a href="" @click.self='handle2'></a>
<!-- 修饰符可以串联的，但是顺序很重要 ，用@click:prevent.self 阻止所有的点击  用@click.self.prevrnt 阻止元素自身的点击-->
```
## 4. 按键修饰符keyup
常用的按钮修饰符
    .enter
    .tab
    .esc
    .delete
    .up
    .down
    .left
    .right
### 4.1 自定义按键修饰符
    声明
    Vue.config.keyCodes.自定义名称 = ASCII
    使用
    @keyup.自定义名称 = 事件函数
## 5. v-if 判断
v-show 与 v-if 的区别
v-if 是在DOM上进行动态的加载和销毁。控制是否渲染到页面上
v-show 是已经渲染到页面上拉，控制的dispaly 的 none 和 block,因此效率比较高。
## 6. v-model 双向数据绑定
什么是双向数据绑定？
当数据发生变化时，视图也跟着变化。
当视图发生变化时，数据也跟着同步变化。
v-model的使用场景
v-model 是一个指令 限制在 <input> <select> <textarea> compenents(组件) 中使用
## 7.v-for 循环
v-for 循环可以循环数组和对象，最好给每个循环对象绑定key.
key是来给每一个节点做一个标识符，key的作用主要是为了高效的更新虚拟DOM.
v-for 最好不要和v-if 一起使用，因为 v-for 的优先级比 v-if 高。
## 8. class绑定数组和绑定对象
绑定对象 :class = {键： 值} 键代表类目，值为true，表示显示
绑定数组 :class = [值1，值2]  值1，值2 对应的 data 中的数据
两个的区别？
绑定对象的时候对象的属性，即要渲染的类名，对象的属性值对应的值对应的是data中的数据，绑定数组的时候数组里面存的是data中的数据。

