# vue 双向数据绑定

## 1、Vue2.X 的数据双向绑定原理

Vue2.X 的数据双向绑定原理：采用的是 Object.defineProperty()方法

```javascript
<!-- 1.Vue2.x采用的数据双向绑定原理 -->
    <input type="text" v-model />
        <p v-bind></p>
        <script>
    	// 1.获取行内属性[v-model]
    	let txt = document.querySelector("[v-model]");
    	// 2.input框实现输入值
    	txt.oninput = function () {
    		// 3.将获取到的值赋给obj.name
    		obj.name = txt.value;
    	};
    	let obj = {};
    	// 4.IE8以及以下浏览器不识别该用法，自带了get和set方法
    	// get的得到值变化，set是设置值的变化
    	Object.defineProperty(obj, "name", {
    		get() {
    			console.log("get被调用了");
    		},
    		// set自带一个参数
    		set(value) {
    			console.log("set被调用了");
    			// 获取到p的属性
    			let p = document.querySelectorAll("[v-bind]")[0];
    			// 赋值
    			p.innerHTML = value;
    		},
    	});
    	// obj.name = 'summer';  // 触发了set
        // console.log(obj.name); // 触发了get
        </script>
```

## 2、Vue3.X 的数据双向绑定原理

Vue3.0 实现数据的双向绑定原理：采用的是 Proxy 方法,proxy 是个对象

```javascript

<input type="text" v-model>
    <p v-bind></p>
    <script>
        let txt = document.querySelector('[v-model]');
        txt.oninput = function(){
           p.obj = txt.value;
        }


        let obj = {};
        let p = new Proxy(obj,{
            // 1.目标对象
            // 2.被获取的属性值
            // 3.Proxy或继承Proxy
            get(data, property, receiver){
                // console.log('get被调用了');
            },
            // 1.目标对象
            // 2.被获取的属性值
            // 3.被获取的value值
            // 4.最初被调用的对象Proxy
            set(data, property, value, receiver){
                // console.log('set被调用了');
                let p = document.querySelectorAll('[v-bind]')[0];
                p.innerHTML = value;
            }
        });

        // p.name = 'summer';  // set被调用了
        // console.log(p.name); // get被调用了

    </script>
```

## 3、proxy 比 Object.defineProperty 好在哪里？

### proxy 优势：
1. 可以直接监听对象而非属性
2. 可以直接监听数组的变化
3. Proxy 有多达 13 种拦截方式，不限于 apply、ownKeys、deleteProperty、has 等等是 Object.defineProperty 不具备的
4. Proxy 返回的是一个新对象，可以只操作新的对象达到目的，而 Object.defineProperty 只能遍历对象属性直接修改

### Object.defineProperty 的优势：
1. 兼容性好，支持 IE9，而 Proxy 的存在浏览器兼容性问题，而且无法用 polyfill 磨平

