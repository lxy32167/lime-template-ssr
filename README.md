

<h2 align="center">lime-template-ssr 服务端渲染模板</h2>

<p align="center">基于 webpack 工作流，采用 Vue SSR 实现服务端同构渲染的 SFB (前后端分离)同构应用</p>

## Feature

* 基于 Babel7 和 webpack4 全面拥抱 ESNext 语法
* 基于 VueSSR 前后端同构，便于复用代码；分离开发，聚合部署
* 可在一个应用内同时支持 SSR、API 开发和传统后端视图渲染
* 支持 static(纯前端渲染) 和 ssr(服务端渲染) 快速切换
* SSR 同时实现了开发模式和生产模式的智能降级策略
* vue-meta 处理 SEO TDKH
* 内置 HMR(hot module replace) 提升开发体验
* source-map 开发环境精确到vue单文件的调试体验
* 基于 koa-static 实现静态资源 cache
* 内置了 LIME 常用的插件，如全局logger函数、bodyParser、ctxlog等，其中 ctxlog() 可打印全息日志(把一个请求的日志收集在一起打印)
* TODO: 前端 code spliting 代码分割 (p0)
* TODO: 使用 ESLINT standard 规范代码 (p1)
* TODO: 后端 api 单测 (p1)
* TODO: 文档 (p1)
* TODO: 发布优化: 静态资源 cdn 化和url自动替换

## Usage

* 立刻开发

    ```bash
    # 安装依赖
    npm install （国内请优先使用淘宝/腾讯镜像源）
    # 开发模式启动。开发模式默认使用 static(纯前端渲染) 
    npm run dev # 其中 PROXY 是指的你的 fiddler 监听地址和端口
    ```

* 切换渲染模式

  为了提升开发体验，开发模式下默认是 static 前端渲染模式；生产环境下默认是 SSR 服务端渲染模式。你可以通过两种方式轻易修改覆盖当前的模式.

  - 方法1： 启动时设置 node 的环境变量 MODE=static/ssr。 其中 static 表示前端渲染，ssr表示后端渲染
  - 方法2：在浏览 URL 后追加 `_mode` 查询字符串: https://boodo.qq.com/?_mode=static/ssr

  注意：在 SSR 模式下，api 请求是从 node层 发出；因此在本地开发时，通常你需要确保node程序自身可以访问到目标接口。如果你的网络需要代理才可访问对方接口，你可以通过: `PROXY=127.0.0.1:8080 npm run dev` 这样的方式来让服务端渲染时可以走代理访问接口。

* 抓包
    - 理论上，对于开发页面本身来说 使用轻量的 Chrome DevTool 开发者工具已经够用
    - 当然，如果你习惯使用 fiddler 或 whistle 抓包，也可以配置浏览器的网络代理来实现 fiddler/whistle 抓包. 例如 fiddler 中可以如下方式配置一个 Extension 规则:

      ```
      Enabled	Match	Action
      Checked	yourdomain.com	x-overrideHost=127.0.0.1:3000 // 这表示让fiddler把 yourdomain.com 这样的域名请求全部转发给本地的3000端口(你启动的node服务)
      ```

* 编译

  ```bash
  npm run build
  ```

  * 发布

  发布时，如果CI支持，请排除掉frontend源码目录及其内部的node_modules(只是被前端开发所依赖)

## Introduction

前后端分离的同构应用在某些方面拓展了前端项目的能力，SSR对爬虫友好度和首屏时间都有一定的改善；语言的同构也带来的前后端代码的复用性。不过，这也给项目结构组织带来了一定的复杂性。


## SEO

项目采用 `vue-meta` 设置页面 tdkh 信息，支持 SSR 和 SPA 两种模式。需要配置页面 meta 信息时，只需在 Vue 组件选项中如下设置:

```js
export default {
  metaInfo() {
    return {
      title: this.baseInfo.name, // 页面标题
      titleTemplate: "", // 标题字符串模板
      // meta 信息
      meta: [
        { name: 'keywords', content: "",
        { name: 'description', content: "" }
      ]
    }
  }
}
```

更多语法请参考: [vue-meta](https://github.com/nuxt/vue-meta)
