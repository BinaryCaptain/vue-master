## 技术栈：
* vue 2.X
* vuex
* vue-router
* element-ui
* axios
* koa2
* mongoose
* jsonwebtoken

## 功能：
用户输入网站进入localhost:8000/,由于没有登录直接跳转到/login页面，登录完成后自动跳转到主页并能进行其他操作。

## 运行环境：
由于用的是koa2,所以请在官网下载最新版本,我用的是**7.8.0**版本。建议下载个nvm，它是window下管理node版本的工具，非常好用，只需几个命令就能随时切换node版本</br>
项目目录是用vue-cli搭建。然后自己在里面新建了server.js和server文件夹来写后端代码。不能少一步就是在config/index.js配置代理
```javascript
proxyTable: {
      '/api': {
				target: 'http://localhost:8888',
				changeOrigin: true
			}
    }
```

## 运行项目
前提条件：mongodb服务是挂起来的
```javascript
//第一步
cd vue-koa2-login
//第二步
cnpm install
//第三步
npm run dev
//第四步:挂起mongodb
mongod --dbpath XXXX(可以随便建个文件夹，这里是该文件夹的地址，将来用来存放数据)
//第五步
node server.js
```

