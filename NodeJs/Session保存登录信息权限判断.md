# Session 保存登录信息路由权限判断

对于所有的路由，用户在没有登录的情况下，不能进入其它路由进行查看操作。这就需要我们对路由的权限进行限制。

```js
//ejs中 设置全局数据   所有的页面都可以使用
//app.locals['userinfo']='1213'
//app.locals['userinfo']='111111';
//  ejs 中直接使用数据 <%=userinfo%>，不需要再传值使用

// 自定义中间件  判断登录状态
app.use(function(req,res,next){

    //console.log(req.url);
    //next();
    if(req.url=='/login' || req.url=='/doLogin'){
        next(); // 继续向下匹配路由
    }else{
        if(req.session.userinfo&&req.session.userinfo.username!=''){   /*判断有没有登录*/
            app.locals['userinfo']=req.session.userinfo;   /*配置全局变量  可以在任何模板里面使用*/
            next();
        }else{
            res.redirect('/login')
        }
    }
})

```

其中  `ejs` 配置全局变量，所有页面都可以使用。

```js
// 定义
app.locals['userinfo']='1213'
```

```html
<%=userinfo%>
```
