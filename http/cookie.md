# cookie

Cookie 用于存储 web 页面的用户信息（储存在用户本地终端上的数据）

cookie是以名值对的形式存在，如下：

```javascript
username=yaodao
```

使用js创建cookie

基本设置

```javascript
document.cookie = 'username=yaodao'
```

添加过期时间

```javascript
document.cookie = `username=yaodao; expires=${new Date(2020).toGMTString()}`
```

添加cookie路径

```javascript
document.cookiew = "username=yaodao; path=/"
```

修改cookie

修改cookie很方便，和创建一样，新的值覆盖旧值

```javascript
document.cookie = 'username=yaodao2'
```

删除cookie

设置过期时间为当前时间

```javascript
document.cookie = `username=ydaodao; expires=${new Date().toGMTstring()}`
```

## Cookie 跨域共享

domain表示的是cookie所在的域，默认为请求的地址，如网址为www.jb51.net/test/test.aspx，那么domain默认为www.jb51.net。而跨域访问，如域A为t1.test.com，域B为t2.test.com，那么在域A生产一个令域A和域B都能访问的cookie就要将该cookie的domain设置为.test.com；如果要在域A生产一个令域A不能访问而域B能访问的cookie就要将该cookie的domain设置为t2.test.com。

### 参考文章

[Document​.cookie
](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/cookie)