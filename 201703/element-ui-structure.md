dependencie://github.com/ElemeFE/element) 是由饿了么前端团队出品基于 [Vue 2.0](https://cn.vuejs.org/) 的组件库，方便开发人员、设计师和产品经理快速搭建页面。目前在 GitHub 上开源，接近 10000 star。

## 目录结构

## element 编译打包后输出什么？

element 的源码以组件为单位来组织，最终编译打包的时候，为了用户可以引入全部组件，会将所有的组件打包成一个 Vue 插件，同时为了方便按需引入，也会将所有的组件分别单独打包成一个 Vue 组件。

```javascript
// 全部引入
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-default/index.css'

Vue.use(ElementUI)

// 按需引入
import { Button } from 'element-ui'
Vue.component(Button.name, Button)
```

全部引入时，需要引入全部样式 `index.css`，同时利用 Vue 的插件机制来导入所有的组件。

如果按需引入，需要借助 [`babel-plugin-component`](https://github.com/QingWei-Li/babel-plugin-component) 来逐个引入组件。`babel-plugin-component` 的作用是将：

```javascript
import { Button } from 'element-ui'
```

转化为：

```javascript
var button = require('components/lib/button')
require('components/lib/button/style.css')
```

所以其实打包的时候还是 js 和 CSS 分开打包的，所以引入一个组件还是要分别引入对应的 js 和 CSS，虽然借助一些插件可以让我们的代码看起来是通过一行代码就引入了一个组件。

## 如何做到 CSS 代码的解耦？

其实最直接的办法就是每个组件的 CSS 代码都分开写，一些公用的样式，比如颜色、动效可以定义为变量，然后再在每个 CSS 文件中引入即可达到复用的目的。element 使用了 [PostCSS](http://postcss.org/)，其实无论用 PostCSS、Sass 还是 less，都是这样的处理思路。

有些组件会依赖其他组件，比如 `Select` 和 `Cascader` 都依赖了 `Input`，所以最后打包的时候 `Select` 和 `Cascader` 的 CSS 中都会包含 `Input` 组件的样式，而不是将 `Input` 打包成一个公共的依赖。个人认为这个设计是合理的，因为用户在使用的时候，单独引入 `Select` 即可，就不需要在引入其他依赖了。如果公用的样式如果单独打包成一个依赖文件，那么用户在使用任何组件之前都需要先引入这个公共依赖，如果用户只使用了一两个组件却引入了所有组件的公共依赖，这会造成更大的浪费。所以在开发过程中，为了复用，组件之间可以有依赖关系，但最后暴露给用户的组件，不要有公共依赖（除 Vue 外），也不要有相互依赖关系。
 
 ```CSS
 /* Select CSS */
 @charset "UTF-8";
 @import "./common/var.css";
 @import "./select-dropdown.css";
 @import "./input.css";
 ...
 
 /* Cascader CSS */
 @charset "UTF-8";
 @import "./input.css";
 @import "./common/var.css";
 ...
 ```

## 一个独立组件的代码结构
