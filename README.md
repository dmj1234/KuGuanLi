### 1、客户登录界面总结 

#### 登录业务的相关技术（存储方式）
- http 是无状态的
- 通过cookie在客户端记录状态
- 通过session在服务器端记录状态  (如果我们没有跨域问题，我们就用cookie和session)
- 通过token方式维持状态  （token由服务器生成的）

#### 按需导入和组件导入的区别
- 主要就是一句话, 如果用到的组件少, 就按需引入, 如果用到的组件很多,就全部引入, 因为按需引入和全部引入效果一样
- 全局导入的特点：使用组件，默认为全部注册，还要导入需要**挂载Vue全局属性**上去，这样会导致项目的体积边大
```js
//第一种引入
import  {Message} from 'element-ui'
Vue.prototype.$message =Message
//$message 自定义属性  但是等号后面的必须是所调用的Message元素   

//第二种引入
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
//使用成功

```
- 按需导入的特点：减少项目体积，但是每当用到element-ui 就需要手动去导入
```js
import { Button } from 'element-ui'
import { Form } from 'element-ui'
import { FormItem } from 'element-ui'
import { Input } from 'element-ui'
Vue.use(Button)
Vue.use(Form)
Vue.use(FormItem)
Vue.use(Input)
//Vue.use() 这种方式注册  引入出了问题，没有渲染出来，全局引入成功了
```
#### 路由导航守卫控制访问权限
- 如果用户没有登录，但是通过URL访问特定页面，需要重新导航到登录页面
```js
//为路由对象，添加beforeEach导航守卫
router.beforeEach((to,form,next) => {
  //如果用户访问的登录页，直接放行
  if (to.path ==='./login') return next()
  //从sessionStorage中获取保存的token值
  const tokenStr = window.sessionStorage.getItem('token')
  //没有token，强制跳转到登录页
  if(!tokenStr) return next('/login')
  next()
})
```
#### 退出功能实现的原理
- 基于token的方式实现退出，只需要销毁本地的token就可以了，这样后续的请求就不会懈怠token，必须重新登录才能生成新的token之后才能访问。
```js
//清空token
window.sessionStorage.clear()
//跳转到登录页面
this.$router.push('/login')
```


