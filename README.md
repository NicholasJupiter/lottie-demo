# 如何使用
[Lottie DOC-Web官网](https://airbnb.io/lottie/#/web)
[Lottie预览JSON动效地址](https://lottiefiles.com/preview)
## 如何安装
```javascript
// 1.使用npm安装
npm i lottie-web
// 2.使用CDN引入
<script src="https://cdnjs.com/libraries/bodymovin"></script>
```
## 加载JSON文件
```javascript
// json path https://uxpinchina.com/voice.json
// index.js
const animation = lottie.loadAnimation({
  container:document.querySelector('#lottie'),
  renderer:'svg/canvas/html',
  path:'data.json',
  loop:true,
  autoplay:true,
  name:'demo',
  animationData:{}
})
```
`container`：用来渲染的dom元素。
`path`：AE或者Lottie导出的JSON文件路径。
`renderer`：`svg`|`canvas`|`html`可以渲染成三种状态
`loop`：`true`|`false`是否循环动画。 `true`->`animation-iteration-count:infinite;`。
`autoplay`：`true`|`false`是否自动播放。
`name`：动画名称。
`animationData`：JSON数据 `path`和`animationData`二选一即可。
## 实例方法
![[code.png]]
- `play()`：启动
- `stop()`：停止
- `pause()`：暂停
- `destory()`：销毁
- `setLocationHref(href:string)`：`href`作为`location.href`，当你在Safari中遇到不带符号的掩码问题时，它非常有用
- `setSpeed(speed:number = 1)`：设置动画速度
- `goToAndStop(value:number,isFrame:boolean = false)`：动画跳到某个进度并停止。
`value`：进度数值
`isFrame`：定义`value`是基于时间`true`还是基于帧`false`
- `goToAndPlay(value:number,isFrame:boolean = false)`：动画跳到某个进度并播放。
- `setDirection(direction:number = 1)`：设置动画的播放顺序。`正数和0`：正序，`负数`：倒序
- `getDuration(inFrames:boolean = false)`：获取动画持续事件。
`true`->返回以帧为单位的持续时间
`false`->返回以秒为单位的持续时间
- `playSegments(segments:[number,number]|[number,number][],forceFlag:boolean = false)`：
  `segments`：参数指定播放帧 `[form,to]`，会停留在最后一个二维数组的`to`里面 。
  `forceFlag`：参数表示是否立即强制播放该片段 。
  `false`->会等待当前段完成。
  `true`->会立即播放此段。 如：
  `animation.playSegments([15,30],false); // 播放完之前的片段，播放15-30帧`，
  `animation.playSegments([[0,5],[10,20]]); // 直接播放0-5和10-20帧`。
 - `setSubframe(boolean = true)`：`false`->将尊重原始的AE fps。`true`->将使用中间值在每个RequestAnimationFrame上更新。

## 事件
-   `complete`: 播放完成（循环播放下不会触发）
-   `loopComplete`: 当前循环下播放（循环播放/非循环播放）结束时触发
-   `enterFrame`: 每进入一帧就会触发，播放时每一帧都会触发一次，stop方法也会触发
-   `segmentStart`: 播放指定片段时触发，`playSegments`、`resetSegments`等方法刚开始播放指定片段时会发出，如果`playSegments`播放多个片段，多个片段最开始都会触发。
-   `data_ready`: 动画数据json文件加载完毕触发
-   `data_fail`：动画数据json文件加载失败触发
-   `loaded_images`：当所有图片加载成功/失败时触发
-   `DOMLoaded`: 动画相关的dom已经被添加到html后触发
-   `destroy`: 将在动画删除时触发
```javascript
// animation 是一个实例对象通过lottie.loadAnimation
animation.addEventlistener('data_ready',res=>{
  // handle
  console.log('animation file has loaded');
})
```
## lottie.json
lottie.json关键字
-   ip：起始帧
-   op：结束帧
-   w：宽
-   h：高
-   v：版本
-   fr：频率/帧率
-   ddd
-   nm
-   tiny
-   fonts：字体
    -   list
        -   fontConfig
-   asset：资源
    -   assetConfig
        -   id
        -   w：宽
        -   h：高
        -   u：路径
        -   p：名称
        -   e
-   layers：图层
    -   layerConfig
        -   st：开始帧
        -   ip：起始帧
        -   op：结束帧
        -   nm：名字
        -   ind：图层id
        -   ty：图层类型
        -   ks：动画
        -   shapes：图形