###  项目遇到困难和技术

app常用背景颜色：说一下



###  小程序图片上传

![1642222442375](C:\Users\ZLY\AppData\Roaming\Typora\typora-user-images\1642222442375.png)

​	wx.chooseImage({})

![1642229333375](C:\Users\ZLY\AppData\Roaming\Typora\typora-user-images\1642229333375.png)

![1642229299707](C:\Users\ZLY\AppData\Roaming\Typora\typora-user-images\1642229299707.png)

wx.uploadFile()

###  小程序小技巧：

​	用一个函数时，想看里面的所有方法，可以wx-request  按照提示来让他显示

​	wx:key="中间放各个循环项中不会重复的属性"

​	小程序提供页面滚动效果：scroll-view组件

​	使用scroll-view时切换页面让他回到顶部，要把scroll-top属性中的值定义为变量，每次都重新赋值

​	如果遍历的是一个简单数组，wx：key=“*this”

​	如果有多层循环，就要给wx：for-item=“item1” 遍历项定义不同的名称

​						   wx：for-index=“index1”

​	小程序内button宽度设置为100%无效问题
​	直接在wxml中的button按钮上写行内样式，将style设为width:100%(推荐这种方法)。	



​	



###  rpx和px之间的转换

​	小程序的默认750rpx是占满全部的，所以说，页面如果是375px，那么

​	1px = 2rpx

​	750/页面大小 = 1px等于多少rpx



###  引入自定义组件

​	在要用的页面js  usingComponents{} 中使用键值对的方式引用

```
"usingComponents": {

    "search":"../../components/search/search"

  }
```



###  让元素内的内容水平垂直居中（flex）

​	让外层元素css加：

​		display：flex；

​		justify-content：center；

​		align-items：center；

​	还可以使元素的主流方向改变，变为上下

​	display:flex;

​	flex-direction:cloumn;

flex元素自动换行：

flex-wrap: wrap;





###  浏览器查看json数据插件

​	jsonview   json-handle



###  小程序发送异步网络请求拿数据	

```
wx.request({
      url: 'http://suggest.taobao.com/sug?code=utf-8&q=商品关键字&callback=cb',
      data:{
        name:"zhangsan"
      },
      method:"GET",
      dataType:"JSON",
      success(res){
        console.log("success：",res);
      },
      fail(res){
        console.log("fail",res);
      }
    })
```



###  把后台请求添加到小程序后台中

​	开发阶段可以使用

![1641712047585](C:\Users\ZLY\AppData\Roaming\Typora\typora-user-images\1641712047585.png)

这种方式来使用，后期要发布时就必须加到小程序后台中  开发管理>开发设置>服务器域名>中添加请求的列表



###  小程序的第三方框架：

基于vue的：腾讯 wepy ，美团 mpvue ， uni-app

基于react的：京东 taro

原生框架：mina





###  less使用技巧：

微信小程序中使用less，项目打包编译时会自动把less文件转为wxss文件（使用calc属性时要注意）

​	height: ~'calc(100vh - 20rpx)'  用~'' 包裹起来的内容不会被编译

​	选择器{

​		属性。。。。

​		它下面的元素标签{

​			属性。。。。

​			它下面的元素标签{

​				属性。。。				

​			}

​		}

​	}



###  css中变量定义和使用

​	变量一般定义在body中，方便做全局使用

​	body{

​		--变量名：值；

​	}

​	使用该变量：

​	选择器{

​		color:var(变量名)；

​	}

### navigator跳转页面传参

​	小程序中navigator也支持url传参

​	在跳转后的页面onLoad（options）函数中，options中获取传过来的参数

### 本地存储

网络请求数据量太大，优化用户体验，做缓存

​	在打开页面，先做 一个判断，判断本地存储中有没有数据并且没有过期，有数据就就用本地的数据，如果没有，就发送新的请求获取新的数据

​	小程序中也有本地存储技术

​	获取本地存储数据（wx.getStorageSync("本地存储中的变量名")）

​	把获取到的数据存储到本地（wx.setStorageSync("存储到本地数据的变量名",数据)） => 其中的数据格式，一般为{time：Date.now()	,data:数据}，存放一个时间，来判断过没过期

![1641785127847](C:\Users\ZLY\AppData\Roaming\Typora\typora-user-images\1641785127847.png)



### 优化网络请求路径

​	把请求中公共的部分提取出来

​	让每次请求拼接一下



###  上拉加载更多，下拉刷新

​	下拉刷新效果：在页面中显示下拉框，在页面json中加上enablePullDownRefresh（他其中的配置去看小程序官方文档），找到下拉刷新的事件在页面生命周期中，然后重置数据，重置页码，重新发送请求，数据请求回来后手动关闭下拉效果，wx.stopPull...这个方法

​	上拉加载更多效果：

​		1.找到滚动条触底事件

​		2.判断有无下一页数据（有下一页数据，加载下一页    ，  没有下一页数据，弹出提示框，没有数据了）

接口要的参数	（里面有当前页数，和页容量）

![1641904664638](C:\Users\ZLY\AppData\Roaming\Typora\typora-user-images\1641904664638.png)



### 加载中图标

​	在数据请求过程中显示加载中图标，数据请求成功后去除这个图标（wx.showLoading()）=>其中mask属性：是否加一层蒙版，用户无法进行其他操作

​	如果一个页面有多个请求，可以在网络请求方法中加一个变量，每请求一次加1，请求结束后减1，最后为0时关闭这个图标



###  点击图片有查看大图的功能

​	调用小程序的api  previewImage



###  css实现文本超出后显示...

![1641906300143](C:\Users\ZLY\AppData\Roaming\Typora\typora-user-images\1641906300143.png)



###  css让一行的元素左右对齐

​	![1641960979128](C:\Users\ZLY\AppData\Roaming\Typora\typora-user-images\1641960979128.png)



###  js中every（）数组方法

![1641961652425](C:\Users\ZLY\AppData\Roaming\Typora\typora-user-images\1641961652425.png)





###  微信支付

​	要做这个功能必须有企业级id

```js
wx.requestPayment({
  timeStamp: '',
  nonceStr: '',
  package: '',
  signType: 'MD5',
  paySign: '',
  success (res) { },
  fail (res) { }
})
```







###  商品收藏功能

![1641975591810](C:\Users\ZLY\AppData\Roaming\Typora\typora-user-images\1641975591810.png)



###  防抖和节流

​	防抖：一般用在搜索中，加定时器，延缓用户频繁发送请求，加个定时器

​	节流：一般用在页面的下拉，和上拉中



###  	小程序后端怎么搭建（就是通过后端操作数据库后返回过来的接口地址，前端通过网络请求来接收数据）





### 微信获取用户信息

​	通过button获取到的是默认信息   ？

​	。。。



###  后端返回的数据如何定义成网络地址

​	？？？



###  总结

​	就比如说，我做小程序，只要返回的数据json格式是我要用的，后端就不用写在项目内，语言也随便选择

​	

​	前端：通过请求获取后端返回的数据，渲染出来

​			和获取用户在页面上的操作通过请求传给后端（例如用户输入账号密码，传给后端，后端获取后比对或者。。。一些操作）



​	es7的async能直接避免回调地狱（网络请求太复杂），有好多机型不适配，如果没有特别要求，就还用es6的promise



​	如果怕一些标签的默认样式影响布局，就把他的透明度变为0，然后大小位置定义到想要的位置。

​	

​	空对象的bool类型也是true



​	时间类型数据转换tolocalString()函数

​	

