# 搜索框

## 组件有两种模式, 一种单张图片模式, 即普通大图模式, 另一种为轮播图模式, 适用于多张图

> 引用并注册组件

``` javascript
  import { CustomizedImgViewer } from './index.vue'
```

> 单张模式

``` html
  let imgSrc = 'http://gss0.baidu.com/94o3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/3b87e950352ac65cf0422d54f9f2b21193138a56.jpg'

  <customized-img-viewer
    :title="查看大图"
    :altText="图片详情"
    :title="图片详情"
    :width="80%"
    :download="false"
    :img-src="imgSrc"
    ref="singleImgViewer">
  </customized-img-viewer>
  
  // 在父组件中点击小图时调用open方法打开组件
  this.$refs.singleImgViewer.open()

```

> 轮播模式

``` html
  let imgArr = [
    'http://gss0.baidu.com/94o3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/3b87e950352ac65cf0422d54f9f2b21193138a56.jpg',
    'http://gss0.baidu.com/94o3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/3b87e950352ac65cf0422d54f9f2b21193138a56.jpg',
    'http://gss0.baidu.com/94o3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/3b87e950352ac65cf0422d54f9f2b21193138a56.jpg'
  ]

  <customized-img-viewer
    :title="查看大图"
    :width="80%"
    :download="false"
    :imgIndex="0"
    :img-src="imgArr"
    ref="mutipleImgViewer">
  </customized-img-viewer>

  // 在父组件中点击小图时调用open方法打开组件
  this.$refs.mutipleImgViewer.open()
```

> 可选参数

| 参数 | 说明 | 类型 | 默认值 |
| :--: | :--: | :--: | :--: |
| title | 模态框的标题 | string | '查看大图' |
| width | 模态框宽度 | string | 80% |
| altText | 单图模式图片alt | string | '图片详情' |
| titleText | 单图模式图片title | string | '图片详情' |
| imgSrc | 图片地址 | string / array | - |
| download | 是否提供下载按钮 | boolean | false |
| imgIndex | 轮播模式下图片索引 | number | 0 |

> 事件

| 方法名 | 说明 | 参数 |
| :--: | :--: | :--: |
| open | 开启模态框 | - |
