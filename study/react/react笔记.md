## react

react 全家桶

es567 语法

node_modul 里面的代码 import 不加路径

antd {} 加结构

空数组

...展开  是新对象 

reduce 需要拷贝对象,重新渲染!!

原来有就覆盖e={

 ... a

c=100

}

const A = function(a){

}

=> 表示函数

const A = () ==> console.log();

一个参数 括弧可以去掉

const B = q => p => p + q ; // 习惯就好

B(100)(200); // 300

箭头函数的this 不会变,没有arguments  ?

如何绑定函数this  后面演示

import _ form 'underscore'

_.unique("数组") 循环

Reduce 聚合

Find 找第一个

Map 循环便利

forEach没有返回值,就不是纯函数, 有副作用 不好控

纯函数可控

let 语法 ` 拼接字符串`

装饰器, 不太重要

异步:

Promise(链式调用解决回调问题 jQuery.then)/Generate(分段执行一个方法)

/async awit (es7 异步调用的地方,await 能够拿到上面请求的结果)

以上三个用的不多,但用法只有一个,所以初期不用纠结, 

Generate 写法非常固定, yield

async server.js 里面用到, 固定语法,后端暂时不care, 一般没问题

注意事项:

 尽量使用===

字符串模版

300行文件 拆组建

推荐使用纯函数,避免bug

谨慎速写全局样式



单页应用,整个系统只有一个页面

导航head不用刷新

不会跳转

HTML history api 更改地址栏

通过nginx 保证刷新后不会404,  打到接口上

DVA(包含三个框架 ): react-router

Redux  Redux-saga 解决支持异步数据传输

babel 装成ie 语法

webpack 打包构建 一般不需要改动

## REACT

函数组件

没有生命周期

无this

类组件

可控生命周期, 有this.status

最好写函数式组件

组件这块没听清 再听下

constructor(props){

​	取代 getInitialStatus

​	this.status=

}

其它地方用this.setStatus后 ie  (这个是异步的)

只要用componentDidMount 请求后端数据

render之后,这里已经有dom函数了

百度地图需要页面上面已经有节点



componentDidUpdate 

componentWillUnmount 很有用,卸载时发生 回到页面页面怎么还在

清除定时器,

组件间通信

!子不要修改props

子到父亲,用callback

兄弟节点数据通信, 同时向父亲节点传递,=》麻烦

用redux

网上方法取代dom操作

link 这块 需要重新听, Link withRouter,

代码里面跳转,BrowserHistory.push

也可以url变 react不跳  spa

reducer 返回的是新对象,这样才能change state  store 没听清. 

免除组件间通信

可以到组件库提交issual 公共组件开发

## 代码阅读



connect mapStaus to props



@connect是语法糖



constructor this 没听清 再听下

:: 意思是 .bind(this)

代码解读 yield 那边没听清

一般item.id都是后端给的

有问题可以问 后端交流

明站讲下一个页面

























rong qi shi























































