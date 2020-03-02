#### 布局用`flex`布局 
> ##### `rn`的`flex-direction`默认值是`"column"`
> ##### `rn`的`align-items`默认值值`"stretch"`
> ##### `rn`的`flex`不可以写复合属性的形式，`web中的写法可以为flex: 2 2 10%`

#### 宽高不用写单位，因为在不同设备上根据不同`dpi`所显示的效果要求要相同

#### 关闭黄色警告
> ##### 关闭全部警告：`console.disableYellowBox = true` 
> ##### 关闭特定警告：`console.ignoredYellowBox = ["Warning:..."]`


---

## API

#### IOS&Android两端通用的API

- ##### Button: 一个简单的跨平台的组件，可以进行一些简单的定制

```
<Button
    onPress={onPressLearnMore} //用户点击此按钮时所调用的处理函数
    title="learn More" //按钮内显示的文本
    color="#000" //文本的颜色，或是按钮的背景色（Android）
    disabled={false} //按钮是否可以点击
    accessibilityLable="Learn more about this purple button" //用于给残障人士显示的文本（比如读屏应用可能会读取这一内容）
/>

```

- ##### ActivityIndicator: 显示一个原型的loading提示符号

```
<view style={[styles.container, styles.horizontal]}>
    <ActivityIndicator
        size="large" //提示器的大小，默认为"small"[enum("small", "large"), number], 目前只能在Android上设定具体的数值
        animating={true} //是否要显示指示器动画，默认为true为显示
        hidesWhenStopped={false} //早animating为false的时候，是否要隐藏指示器（默认为true），如果animating和hidesWhenStopped都为false，则显示一个静止的指示器。
        color="#000" //滚轮的前景颜色（默认为灰色）。
    />
</View>
```

- ##### Image: 用于显示多种不同类型图片的React组件，包括网络图片、静态资源、临时的本地图片、以及本地磁盘上的图片（如相册）等。
> ##### 下面的例子分别演示了如何显示从本地缓存、网络甚至是以`data:`的base64URI形式提供的图片。 
>> ##### 引入本地图片可以不设置宽高尺寸（图片多大显示多大）
>> ##### 引入网络图片或者base64格式的图片必须给尺寸大小，否则不显示

##### ~IOS默认支持GIF和WebP格式图片~

##### ~Android想要支持需要设置，需要在~`android/app/build.gradle`~文件中根据需要手动添加一下模块：~

```
dependencies {
    //需要支持GIF动图
    compile `con.facebook.fresco:animated-gif:1.10.0`
    
    //支持WebP格式，包括WebP动图(只需要支持WebP格式不需要动图的话，只设置第二句就可以)
    compile `com.facebook.fresco:animated-webp:1.10.0`
    compile `com.facebook.fresco:webpsupport:1.10.0`
}
```

-  ##### SafeAreaView: 解决全面屏适配的问题

-  ##### Text：文本组件（做打点有对应的属性）
u
- ##### TextInput：文本输入组件（必须给边框和边框颜色才可以显示出来）

- ##### View: 容器组件

- ##### WebView：网页的容器（可以引入其他网页页面）

- ##### ListView: 不推荐使用（性能太低，会出现白屏现象）

- ##### VirtualizedList：可查看文档了解

- ##### FlatList、SwipeableFlatList、SectionList：这三个是高性能组件，可以在开发中使用