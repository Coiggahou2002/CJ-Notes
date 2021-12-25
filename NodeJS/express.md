# Express

## 项目脚手架

```shell
mkdir myapp && cd myapp
npx express-generator
```

然后会自动创建项目骨架

## 路由

Route definition takes the following structure:

```javascript
app.METHOD(PATH, HANDLER)
```

Where:

+ `app` is an instance of `express`.
+ `METHOD` is an [HTTP request method](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods), in lowercase.
+ `PATH` is a path on the server.
+ `HANDLER` is the function executed when the route is matched.