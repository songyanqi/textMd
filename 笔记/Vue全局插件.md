### 开发VUE 全局插件：

#### 定义全局插件 plugins.js
Vue.js 的插件应当有一个公开方法 install 。这个方法的第一个参数是 Vue 构造器，第二个参数是一个可选的选项对象：
```javascript
export default {
    install (Vue, options) {
        // 具体4种方式，写在此处
    }
}
```
### main.js里引入并使用
```javascript

import plugins from './plugins'
Vue.use(plugins)

```

### 4种方式：(VUe.minxin 方式、Vue.prototype 方式、Vue.filter方式、Vue.directive 方式)
1、Vue.mixin 方式
注册全局混合对象
注：请谨慎使用全局混入，因为它会影响每个单独创建的 Vue 实例 (包括第三方组件)。
```javascript

Vue.mixin({
    data () {
        return {
            someValue1: 'some value1：mixin的data里的值'
        }
    }
})

```

2、Vue.prototype 方式
定义 Vue 原型上的属性

```javascript

Vue.prototype.someValue2 = 'someValue2：Vue.prototype上的值'
Vue.prototype.getDate = function () {
    let dateNew = new Date()
    return dateNew
}
```
3、Vue.filter 方式
定义全局过滤器
```javascript

// msg 传参
Vue.filter('vcntFormat', function (cnt , msg) {
    return cnt >= 100000 ? Math.floor(cnt / 10000) + '万' : cnt
})

```

(1).使用方法：
<p>{{ name | vcntFormat }}</p>

(2).过滤器传参： 
<p>{{ name | vcntFormat( ' msg' ) }}</p>

(3).过滤器也可以串写多个：过滤器串写之后从前往后执行 然后把结果显示出来
demo:

<p>{{ name | nameShow( ' msg' ) | nameTest}}</p>

```javascript

Vue.filter( ' nameShow ' , function (data,msg) { 
  return name + msg
})
Vue.filter( ' nameTest ' , function (data) { 
  return name + 666
})

```

(4).定义私有过滤器
2个条件： 过滤器名称 和 处理函数
```javascript

export default (
    data:{},
    mehods:{}
    filters:{
        nameShow(){
            return name + ' 私有过滤器 '
        }
    }
)
```

4、Vue.directive 方式
定义全局自定义指令

```javascript
Vue.directive('myfocus', {
    // 当绑定元素插入到 DOM 中。
    inserted: function (el) {
        // 聚焦元素
        el.focus()
    }
})

```