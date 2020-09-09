# jQuery 发送 ajax 请求

## 1、发送请求方法

1. \$get
2. \$post
3. \$ajax

### 1.1、\$.get()

```js
$.get(url地址, [参数]，[回调函数])
```

### 1.2、$.post()

```js
$.post(url地址，[参数]，[回调函数])
```

### 1.3 $.ajax()

```js
$.ajax({
    type: 请求方法,
    url: 请求地址,
    data: 参数,
    success: function(res) {
        console.log(res)  // 成功回调
    }
})
```