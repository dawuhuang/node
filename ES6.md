# ES6
## 1. let 声明变量
1. 块级作用域
2. 暂时性死区，外部不能访问let作用域中的变量
3. 不存在变量提升
## 2. const 声明常量
1. 块级作用域
2. 暂时性死区
3. 声明常量时必须给常量赋值，赋值后不允许在修改值(变量地址)，简单数据类型，不能修改值，复杂数据类型不能修改地址。
## 3. 数组方法
### 3.1 find()
用于找出第一个符合条件的成员，没找到则返回 undefined
```js
arr.find((item,index) => return itme > 9 )   // 找出第一个符合大于9的值
```
### 3.2 findIndex()
用于找出第一个符合条件的成员的位置，没找到则返回 -1
```js
arr.findIndex((item,value) => return item > 9 ) // 找出第一个符合大于9的值的位置
```
### 3.3 Array.from()
将伪数组转换为真正的数组，也可以接受第二个参数，作用类似于map(映射)
```js
Array.form(lis) // lis 是个伪数组，返回值的真正的数组
Array.form(lis, item => itme * 2)  // 返回的是一个值*2的数组
```
### 3.4 include()
查询某个值是否在数组内，返回值是布尔值
```js
[1,2,3,4].includes(3) // true
``` 
### 3.5 扩展运算符
```js
let arr = [1,2,3,4,5,6]
console.log(...arr)  // 1,2,3,4,5,6
```
## 4. 字符串方法
### 4.1 startsWith 和 endsWith
startsWith 用于字符串的开头匹配，返回布尔值
endsWith 用于字符串的结尾匹配，返回布尔值
```js
let str = "hello world!";
str.startsWith("hello"); // true
str.endsWith("!"); // true
```
### 4.2 repeat
将字符串重复 n 次，返回新的字符串
```js
"x".repeat(3); // xxx
```
## 5. Set 数据结构
Set 结构中的数据都是唯一的，不能重复。
1. add() 增加
2. delete() 删除,返回布尔值
3. has() 查询 Set 结构中是否存在某个值，返回布尔值
4. clear() 清空 没有返回值
5. forEach()  遍历
```js
let arr = new Set([1, 2, 3, 4, 5, 1, 2]); // 返回值[1,2,3,4,5]
console.log(arr.add(6));                  // [1,2,3,4,5,6]
console.log(arr.delete(1));               // [2,3,4,5,6]
console.log(arr.has(1));                  // false
console.log(arr.has(2));                  // true
console.log(arr.clear());                 // 清空
arr.forEach(value => {
    return console.log(value);
})
```
## 6. 解构赋值和剩余参数
数组解构
```js
let [name,age] = ['zs',20]
console.log(name) // zs
console.log(age)  // 20
```
对象结构，对象结构时可以取别名。
```js
let {name,age} = {name:'zs',age:20}
console.log(name)  // zs
console.log(age)  // 20
```
剩余参数的值是一个数组...a1
```js
function fn(a1,...a2) {
    console.log(a1)  // 1
    console.log(a2) // [2,3,4,5,6]
}
fn(1,2,3,4,5,6)
```
## 7. 箭头函数
箭头函数的this指向的，简单的来说，定义箭头函数中的作用域this指向谁，它就指向谁。
使用箭头函数的好处：不绑定this.