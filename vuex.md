# Vuex 基础入门

## Vuex 是什么

Vuex 是实现组件全局状态(数据)管理的一种机制，可以方便的实现组件之间数据的共享。
数据可以通过 store 集中来进行全局共享。不使用 vuex 进行数据共享时，数据会在子组件和父组件之间多层的传递。

## 使用 Vuex 统一管理的状态的好处

1. 能够在 vuex 中集中管理共享的数据，易于开发和后期维护。
2. 能够高效地实现组件之间的数据共享，提供开发效率。
3. 存储在 vuex 中的数据都是响应速的，能够实时保持数据和页面的同步。
   一般来说，只有组件之间共享的数据，才有必要存储到 vuex 中；对于组件中的私有数据，存储在组件自身的 data 中即可。但是想全局存储也可以

## Vuex 的基本使用

```js
// 1 安装vuex依赖包
    npm install vuex --save
// 2 导入vuex包，
    import Vuex from 'vuex'
    Vue.use(Vuex)
// 3 创建store对象
    const store = new Vuex.Store({
        // state 中存放的就是全局共享的数据
        state: {count: 0}
    })
// 将store对象挂载到vue实例中
    new Vue({
        store,
        render: h => h(App)
    }).$mount('#app')

```

vue 项目中，一般是在 store.js 文件中导入，并创建对象。在 Vue-cli 脚手架中搭建项目时，选择了 Vuex，会在生成的模板文件中生成 store 文件夹，里面有 index.js。已经导入好包(2)、创建好对象(3)和挂载到实例(4)。将创建的共享数据对象，挂载到 vue 实例中，所有的组件，就可以直接从 store 中获取全局的数据了。

```js
// store文件夹中的index.js
import Vue from "vue";
import Vuex from "vuex";

Vue.use(Vuex);

export default new Vuex.Store({
	state: {},
	mutations: {},
	actions: {},
	modules: {},
});
```

## Vuex 的核心概念

Vuex 中的主要核心概念是：State、Mutation、Action、Getter

### 1. State

State 提供唯一的公共数据源，所有共享的数据都要统一放到 Store 的 State 中进行存储

```js
// 创建store数据源，提供唯一公共数据
const store = new Vuex.Store({
	state: { count: 0 },
});
```

组件想访问到 State 中数据，如想访问到 count
第一种方式：

```js
this.$store.state.全局数据名称;
```

第二种方式：通过导入的 MapmapState 函数，将当前组件需要的全局数据，映射为当前组件的 computed 计算属性，就可以直接调用了。

```js
// 1、从Vuex中按需导入mapState函数。在 `export default`前面导入
import { mapState } from 'vuex'
// 2、将全局数据，映射为当前组件的计算属性
computed: {
    ...mapState({'count'})
}
```

### 2. Mutation

Mutation 用于变更 Store 中的数据，只有 mutation 才能修改数据。

1. 只能通过 mutation 变更 Store 数据，不可以直接操作 Store 中的数据。(意思是不要在其他位置直接对 Stor 的数据进行修改)
2. 通过这种方式虽然操作起来稍微繁琐一些，但是可以集中监控所有数据的变化
   `在触发时不携带参数`

```js
// 定义Mutation
const store = new Vuex.Store({
	state: {
		count: 0,
	},
	mutations: {
		add(state) {
			// 变更状态
			state.count++;
		},
	},
});
```

在**组件**中触发 mutations 修改数据

```js
methods: {
    handle1() {
        // 触发 mutations的，不带参数的
        this.$store.commit('add')
    }
}
```

`在触发时携带参数`

```js
// 定义Mutation
const store = new Vuex.Store({
	state: {
		count: 0,
	},
	mutations: {
		addN(state, step) {
			// 变更状态
			state.count += step;
		},
	},
});
```

在**组件**中触发 mutations 修改数据

```js
methods: {
    handle1() {
        // 在调用commit函数，触发mutations时携带参数
        this.$store.commit('addN',3)
    }
}
```

上面的带参数和不带参数的时触发 mutations 的第一种方式，触发 mutations 的第二种方式：通过导入 mapMutions 函数,将需要的 mutations 函数,映射为当前组件的 methods 方法。

```js
// 1. 从vuex中按需导入mapMutatuons函数
import { mapMutations } from 'vuex'
// 2. 将指定的mutations函数，将需要的mutations函数，映射为当前组件的methods方法。下面就能通过this使用
methods: {
    ...mapMutations(['add','addN'])
    btnHandler2() {
      this.sub()
    },
    btnHandler3() {
      this.subN(3)
    }
}
```

**state 和 mutation 的差遍**
state 导入的是 mapState 函数，映射为当前组件的 computed 计算属性。
mutations 导入的是 mapMutions 函数，映射为当前组件的 methods 方法。

### 3. Action

Action 用于处理异步任务 (在 action 中，不能直接修改 state 中的数据，必须通过 context.commit() 触发某个 mutation 才行。)
如果通过异步操作（比如延时操作）变更数据，必须通过 Action，而不能使用 Mutation，但是 Action 中还是要通过触发 Mutation 的方式间接变更数据。

```js
// 在store的index.js中定义Action
const store = new Vuex.Store({
	// ... 省略其他代码
	mutations: {
		add(state) {
			state.count++;
		},
	},
	actions: {
		addAsync(context) {
			// 延时1秒再进行操作
			setTimeout(() => {
				context.commit("add");
			}, 1000);
		},
	},
});
```

组件中的使用方式，通过 dispatch 触发。

```js
// 触发Action
methods: {
    handle() {
        // 触发action的第一种方法 (不带参数的)
        this.$store.dispatch('addAsync')
    }
}
```

触发 actions 异步任务时携带参数：

```js
// 定义Action
const store = new Vuex.Store({
    // ...省略其他代码
    mutation: {
        addN(state,step) {
            state.count += step
        }
    },
    actions: {
        addNAsync(context.step) {
            setTimeout(() => {
                context.commit('addN',step)
            },1000)
        }
    }
})
```

组件触发 Action

```js
methods: {
    handle() {
        // 再调用dispatch函数，触发actions时携带参数。
        this.$store.dispatch('addNAsync',5)
    }
}
```
this.$store.dispatch() 是触发actions的第一种方式，触发actions的第二种方式：通过刚才导入的mapActions函数，将需要的actions函数，映射为当前组件的methods方法：
```js
// 1. 从vuex中按需导入mapActions函数
import { mapActions } from 'vuex'
// 2. 将指定的actions函数映射为当前组件的methods函数
methods: {
    ...mapActions(['addAsync','addNASync'])
}
```
### 4. Getter 
Getter 用于对Store中的数据进行加工处理形成新的数据，不会影响原来的数据。
1. Getter 可以对Store中已有的数据进行加工处理形成新的数据，类型Vue的计算属性
2. Store 中数据发送变化，Getter 是数据也会跟着发送变化。
```js
// 定义 Getter
const store = new Vuex.Store({
    state: {
        count: 0
    },
    getters: {
        showNum: state => {
            return `当前最新的数量是【'+ state.count +'】`
        }
    }
})
```
使用getters的第一种方式：
this.store.getters.名称
使用getter的第二种方法：
```js
import { mapGetters } from 'vuex'
computed: {
    ...mapGetters(['showNum'])
}
```

