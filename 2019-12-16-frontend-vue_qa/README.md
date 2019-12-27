# Vue QA

## [Vue](https://vuejs.bootcss.com/v2/guide/)

### v-bing 和 v-on

v-bind 指令的缩写是 `:`，用于动态绑定一个或多个属性，或者是一个组件的 `prop`。当 v-bind 绑定的属性是 `class` 或 `style` 的时候，表达式的值可以是数组或对象。代码示例和修饰符描述请见 [官方文档](https://vuejs.bootcss.com/v2/api/#v-bind)。

v-on 指令的缩写是 `@`，用于绑定事件监听器。v-on
被用于普通元素上时只能监听 **原生 DOM 事件**，被用于自定义组件元素上时可以监听子组件触发的 **自定义事件** （由 `$emit` 定义的事件）。v-on 指令支持的修饰符有 `.stop`、`.prevent`、`.once` 等等，代码示例和更多修饰符描述请见 [官方文档](https://vuejs.bootcss.com/v2/api/#v-on)。

### `watch` 和 `computed`

Vue 实例的 `watch` 属性用于监听实例对象上的数据，代码示例请见 [官方文档](https://vuejs.bootcss.com/v2/api/#watch)。

Vue 实例的 `computed` 属性用于计算和生成实例对象的数据，代码示例请见 [官方文档](https://vuejs.bootcss.com/v2/api/#computed)。

### 父组件通过 `prop` 传递数据给子组件

在 Vue 中可以使用 `prop` 属性给子组件传递数据，代码示例请见 [官方文档](https://vuejs.bootcss.com/v2/guide/components.html#通过-Prop-向子组件传递数据)。

### 子组件通过 `emit` 反馈事件给父组件

在 Vue 中可以使用 `emit` 属性给父组件反馈事件，代码示例请见 [官方文档](https://vuejs.bootcss.com/v2/guide/components.html#通过事件向父级组件发送消息)

### 使用 \<slot\> 分发内容

在 Vue 中可以使用 `<slot>` 元素实现父子组件之间的内容分发，[官方文档](https://vuejs.bootcss.com/v2/guide/components-slots.html) 提供了一份精简的代码示例，完善之后的内容如下：

NavigationLink.vue

```vue
<template>
  <a v-bind:href="url" class="nav-link">
    <slot></slot>
  </a>
</template>

<script>
export default {
  name: "NavigationLink"
};
</script>
```

App.vue

```vue
<template>
  <navigation-link url="/profile">
    Your Profile
  </navigation-link>
</template>

<script>
import NavigationLink from "./NavigationLink.vue";
export default {
  name: "App",
  compoments: {
    NavigationLink
  }
};
</script>
```

在示例代码中，App.vue 中使用 NavigationLink.vue 的内容最终会被渲染成

```vue
<template>
  <a v-bind:href="url" class="nav-link">
    Your Profile
  </a>
</template>
```

### 访问元素和组件

在 Vue 中可以使用 `$root` 属性访问根实例，使用 `$parent` 从子组件访问父组件，配合使用 `ref` 属性和 `$refs` 属性从父组件访问子组件。更多访问元素和组件的内容请见 [官方文档](https://vuejs.bootcss.com/v2/guide/components-edge-cases.html#访问元素-amp-组件)。

### 响应式实现原理

ECMAScript5 的 JavaScript 对象可以使用 getter/setter 方法替代属性的定义，示例代码如下：

```javascript
const obj = {
  get name() {
    return "FantasticMao";
  }
};
window.console.log(obj.name); // FantasticMao
```

《JavaScript 权威指南》第六章中将 JavaScript 普通对象的属性称为 **数据属性**，将由 getter/setter 定义的属性成为 **存储器属性**。**数据属性** 拥有一个名字和四个特性：值（value）、可写（writable）、可枚举（enumerable）、可配置（configurable），**存储器属性** 缺少了值和可写的特性，但增加了读取（get）、写入（set）的特性。

ECMAScript5 中定义了一个属性描述符（property descriptor）对象，用于实现上述特性的查询和设置操作。可以通过调用 [`Object.getOwnPropertyDescriptor()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor) 和 [`Object.getOwnPropertyDescriptors()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptors) 获取某个对象的属性描述符，通过调用 [`Object.defineProperty()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty) 和 [`Object.defineProperties()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperties) 设置某个对象的属性和属性描述符。

Vue 在内部通过调用 `Object.defineProperty()` 把 `data` 定义的 **数据属性** 转换成 **存储器属性**，以此来追踪依赖，实现在属性被访问和修改时通知变化。简单版本的实现代码请见 [mvvm.html](./mvvm.html)，更多关于响应式原理的内容请见 [官方文档](https://vuejs.bootcss.com/v2/guide/reactivity.html)。

## [Vue Devtools](https://github.com/vuejs/vue-devtools)

### 查看 Vue 组件

![image](vue-devtools-1.png)

### 查看 Vue 自定义事件

![image](vue-devtools-2.png)

### 查看 Vue Router 路由

![image](vue-devtools-3.png)

## [Vue Router](https://router.vuejs.org/zh/)

### hash 模式 和 history 模式

Vue Router 的 hash 模式使用 URL 的 hash 来模拟一个完整的 URL，为了实现当 URL 改变时，页面不会重新加载。hash 模式的 URL 类似于 `localhost:8080/#/home`、`localhost:8080/#/about`。

Vue Router 的 history 模式使用 `history.pushState` API 实现和 hash 模式一样的功能。并且，history 模式的 URL 不会再带有 `#` 符号，URL 会类似于 `localhost:8080/home`、`localhost:8080/about`。

当使用 Vue Router 的 history 模式时，可能会导致访问路由时后端返回 HTTP 404，此时需要后端额外支持：如果 URL 匹配不到任何静态资源，则应该返回同一个 index.html 页面。更多详细内容请见 [官方文档](https://router.vuejs.org/zh/guide/essentials/history-mode.html)。

## [Vue CLI](https://cli.vuejs.org/zh/)

### [vue-cli-service](https://cli.vuejs.org/zh/guide/cli-service.html#使用命令)

Vue CLI 的 `@vue/cli-service` 安装了一个名为 `vue-cli-service` 的命令。

- `vue-cli-service serve` 命令会启动一个开发服务器（基于 [webpack-dev-server](https://github.com/webpack/webpack-dev-server)），并附带开箱即用的模块热重载。可以通过 `vue-cli-service serve [options]` 选项或者 `vue.config.js` 配置文件的 [`devServer`](https://cli.vuejs.org/zh/config/#devserver) 属性配置开发服务器。通过配置文件的示例如下：

  ```javascript
  module.exports = {
    devServer: {
      open: true,
      host: "0.0.0.0",
      port: 9090,
      https: false
    }
  };
  ```

- `vue-cli-service build` 命令默认会在 `dist/` 目录产生一个可用于生产环境的包，带有 JS/CSS/HTML 的压缩，和为更好地缓存而做的自动的 vendor chunk splitting。

- `vue-cli-service inspect` 命令可以审查当前 Vue 项目的 webpack 配置。

### [配置 webpack](https://cli.vuejs.org/zh/guide/webpack.html)

在 Vue CLI 中可以使用 `vue.config.js` 的 `configureWebpack` 属性配置 webpack。

### [模式](https://cli.vuejs.org/zh/guide/mode-and-env.html#模式)

在 Vue CLI 中默认有以下三种模式：

- `development` 被用于 `vue-cli-service serve`
- `production` 被用于 `vue-cli-service build` 和 `vue-cli-service test:e2e`
- `test` 被用于 `vue-cli-service test:unit`

每种模式都会将 `NODE_ENV` 环境变量的值设置为模式的名称，例如在 `development` 模式下 `NODE_ENV` 的值会被设置为 `"development"`。

### [环境变量](https://cli.vuejs.org/zh/guide/mode-and-env.html#在客户端侧代码中使用环境变量)

在 Vue CLI 中可以使用项目根目录下的 `.env` 和 `.env.[mode]` 文件定义不同模式下的环境变量，这些环境变量会被载入并对 `vue-cli-service` 所有命令、插件、依赖可用。

需要注意的是，只有 `VUE_APP_` 开头的变量会被 `webpack.DefinePlugin` 静态嵌入到 Vue 客户端侧的包中。并且，Vue CLI 定义了两个全局的变量：`NODE_ENV` 和 `BASE_URL`。
