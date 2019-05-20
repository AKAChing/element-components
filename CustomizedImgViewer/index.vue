<template>
  <el-dialog
    :title="title"
    :width="width"
    top="3vh"
    :append-to-body="true"
    :visible.sync="display">
    
    <img
      v-if="Object.prototype.toString.call(imgSrc) == '[object String]'"
      :key="imgSrc"
      :src="imgSrc"
      :alt="altText"
      :title="titleText"
      class="bigImg"
      @click="close">

    <el-carousel
      v-if="Object.prototype.toString.call(this.imgSrc) == '[object Array]' && display"
      indicator-position="outside"
      ref="carousel"
      :autoplay="false"
      :interval="0"
      :initial-index="initialIndex"
      height="900px">
      <el-carousel-item
        v-for="(src,index) in imgSrc"
        :key="index">
        <img v-lazy="src" :key="src" class="carouselImg" :alt="src" :title="src">
      </el-carousel-item>
    </el-carousel>
    <el-button size="mini" type="primary" class="download" v-if="download" @click="downloadImg">下 载</el-button>
  </el-dialog>
</template>

<script>
export default{
  name: 'customized-img-viewer',
  data(){
    return {
      display: false,
      initialIndex: 0,
    }
  },
  props: {
    title: { // 模态框标题
      type: String,
      default: '查看大图'
    },
    altText: { // 图片alt
      type: String,
      default: '图片详情'
    },
    titleText: { // 图片title
      type: String,
      default: '图片详情'
    },
    imgSrc: { // 图片地址 可以使单张图片也可以是图片数组集合
      type: [String, Array],
      required: true
    },
    width: { // 模态框宽度
      type: String,
      default: '80%'
    },
    download: { // 是否提供下载按钮
      type: Boolean,
      default: false
    },
    imgIndex: { // 轮播图下图片的索引
      type: Number,
      default: 0
    }
  },
  watch:{
    imgIndex(v){ // 监听imgIndex 并进行对应操作
      this.initialIndex = v
    }
  },
  methods:{
    /**
     * imgSrc如果为String类型则只有一张图,为Array则为多张图,
     * String时只需要传入对应图片的url地址即可
     * Array时为一个字符串数组,根据需要可以额外传入一个img-index来指定索引
     */
    open(){ // 打开当前组件
      this.display = true
    },
    close(){ // 关闭当前组件
      this.display = false
    },
    downloadImg(){ // 在新窗口打开指定图片
      window.open(this.imgSrc)
    },
    next(){ // 轮播图下一张
      this.$refs.carousel.next()
    },
    prev(){ // 轮播图上一张
      this.$refs.carousel.prev()
    },
    addEvLis(e){ // 轮播图模式下左右箭头绑定对应事件
      if(e.keyCode == '37'){
        this.$refs.carousel && this.$refs.carousel.prev()
      }
      if(e.keyCode == '39'){
        this.$refs.carousel && this.$refs.carousel.next()
      }
    }
  },
  created(){ // 组件创建时绑定键盘监听事件
    document.body.addEventListener('keyup', this.addEvLis)
  },
  mounted(){},
  destroyed(){ // 组件销毁时移除键盘监听事件
    document.body.removeEventListener('keyup', this.addEvLis)
  }
}
</script>
<style lang="scss" scoped>
  .bigImg{
    width: 100%;
    height: auto;
    display: block;
    cursor: zoom-out;
  }
  .carouselImg{
    width: auto;
    height: auto;
    max-width: 100%;
    max-height: 100%;
    margin: 0 auto;
    display: block;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
  }
  .download{
    margin: 20px auto 0 auto;
    display: block;
  }
</style>