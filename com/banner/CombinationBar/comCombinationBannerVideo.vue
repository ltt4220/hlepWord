<template>

<div class="zm-component-combination">  
    <!-- 图片 -->
    <div @click.stop="bannerOneClick" class="zm-component-banner" :class="{nowIsSelect:items[index].nowIsSelect}" data-type-banner="image" v-if="items[index].type=='img'" :style="{width: style.width,height: style.height}">
        <div class="zm-bg-banner-PIC" :style="{backgroundImage:items[index].previewData?'url('+items[index].previewData.poster+')' : 'url('+url+')',backgroundColor:items[index].bottom}"></div>
        <div class="zm-bg-banner-vein" :style="{backgroundImage:items[index].vienImg,backgroundColor:items[index].coverage}"></div>
        <div class="zm-banner-resizeL" v-show='items[index].resizeLShow' @mousedown.stop="resizedown('left')" ><svg xmlns="http://www.w3.org/2000/svg" version="1.1" viewBox="0 20 180.1 212.7" preserveAspectRatio="none"><g><polyline points="56.4,89.8 90.4,56.5 123.7,89.8"></polyline><polyline points="123.7,172.8 89.7,206.1 56.4,172.8"></polyline><path d="M31.1,113.4 149,113.4"></path><path d="M31.1,148.2 149,148.2"></path></g></svg></div>
        <div class="zm-banner-resizeR" v-show='items[index].resizeRShow' @mousedown.stop="resizedown('right')"><svg xmlns="http://www.w3.org/2000/svg" version="1.1" viewBox="0 20 180.1 212.7" preserveAspectRatio="none"><g><polyline points="56.4,89.8 90.4,56.5 123.7,89.8"></polyline><polyline points="123.7,172.8 89.7,206.1 56.4,172.8"></polyline><path d="M31.1,113.4 149,113.4"></path><path d="M31.1,148.2 149,148.2"></path></g></svg></div>
        <div class="banner-Column-label" v-show='items[index].ColumnLabelShow'>第 {{index + 1}}列&nbsp;&nbsp;&nbsp;宽 : {{parseInt(parseFloat(items[index].width)/100*totalWidth)}}px</div>
    </div>

    <!-- 颜色 -->
    <div @click.stop="bannerOneClick" class="zm-component-banner" :class="{nowIsSelect:items[index].nowIsSelect}" data-type-banner="color" v-if="items[index].type=='color'" :style="{width: style.width,height: style.height}">
        <div class="zm-bg-banner-BGC" :style="{backgroundColor:items[index].previewData?items[index].previewData.color : rgb}"></div>
        <div class="zm-banner-resizeL" v-show='items[index].resizeLShow' @mousedown.stop="resizedown('left')" ><svg xmlns="http://www.w3.org/2000/svg" version="1.1" viewBox="0 20 180.1 212.7" preserveAspectRatio="none"><g><polyline points="56.4,89.8 90.4,56.5 123.7,89.8"></polyline><polyline points="123.7,172.8 89.7,206.1 56.4,172.8"></polyline><path d="M31.1,113.4 149,113.4"></path><path d="M31.1,148.2 149,148.2"></path></g></svg></div>
        <div class="zm-banner-resizeR" v-show='items[index].resizeRShow' @mousedown.stop="resizedown('right')"><svg xmlns="http://www.w3.org/2000/svg" version="1.1" viewBox="0 20 180.1 212.7" preserveAspectRatio="none"><g><polyline points="56.4,89.8 90.4,56.5 123.7,89.8"></polyline><polyline points="123.7,172.8 89.7,206.1 56.4,172.8"></polyline><path d="M31.1,113.4 149,113.4"></path><path d="M31.1,148.2 149,148.2"></path></g></svg></div>
        <div class="banner-Column-label" v-show='items[index].ColumnLabelShow'>第 {{index + 1}}列&nbsp;&nbsp;&nbsp;宽 : {{parseInt(parseFloat(items[index].width)/100*totalWidth)}}px</div>
    </div>

    <!-- 视频 -->
    <div @click.stop="bannerOneClick" class="zm-component-banner" :class="{nowIsSelect:items[index].nowIsSelect}" data-type-banner="video" v-show="items[index].type=='video'" :style="{width: style.width,height: style.height}">
        <div class="zm-bg-banner-Video">
            <video :src="items[index].previewData?items[index].previewData.src : src" :poster="items[index].previewData?items[index].previewData.poster : ''" autoplay="true"  ref="video" loop="true"></video>
        </div>
        <span v-show="svgShow" class="span-video iconfont icon-videoBgyulan"></span>
        <div class="zm-bg-banner-vein" :style="{backgroundImage:items[index].vienImg,backgroundColor:items[index].coverage}"></div>
        <div class="zm-banner-resizeL" v-show='items[index].resizeLShow' @mousedown.stop="resizedown('left')" ><svg xmlns="http://www.w3.org/2000/svg" version="1.1" viewBox="0 20 180.1 212.7" preserveAspectRatio="none"><g><polyline points="56.4,89.8 90.4,56.5 123.7,89.8"></polyline><polyline points="123.7,172.8 89.7,206.1 56.4,172.8"></polyline><path d="M31.1,113.4 149,113.4"></path><path d="M31.1,148.2 149,148.2"></path></g></svg></div>
        <div class="zm-banner-resizeR" v-show='items[index].resizeRShow' @mousedown.stop="resizedown('right')"><svg xmlns="http://www.w3.org/2000/svg" version="1.1" viewBox="0 20 180.1 212.7" preserveAspectRatio="none"><g><polyline points="56.4,89.8 90.4,56.5 123.7,89.8"></polyline><polyline points="123.7,172.8 89.7,206.1 56.4,172.8"></polyline><path d="M31.1,113.4 149,113.4"></path><path d="M31.1,148.2 149,148.2"></path></g></svg></div>
        <div class="banner-Column-label" v-show='items[index].ColumnLabelShow'>第 {{index + 1}}列&nbsp;&nbsp;&nbsp;宽 : {{parseInt(parseFloat(items[index].width)/100*totalWidth)}}px</div>
    </div>
</div>

</template>
<script>
export default {
  props: ['dataList','index','widthList','items','totalWidth','backgroundArr'],
  data () {
    return {
      svgShow:true,
      src: 'http://image.zuma.com/upload/968828205866584890.mp4',
      style: {
          width: '100%',
          height: '100%',
      },
      dx:0,
      dw:0,
      totalPer:0,
      totalTwoWidth:0,
      nowElWidth:0,
      nowElWidth2:0,
      nextElPer:0,
      nextElWidth2:0,
      preElWidth:0,
      preElWidth2:0,
      preElPer:0
    }
  },
  computed:{
  },
  methods:{
    bannerOneClick(){
      console.log("nowclick$$$$$$$$$$$$$$$$$$$$$$$$$$$",this.index)
      let obj = {type: this.backgroundArr[this.index].previewData?this.backgroundArr[this.index].previewData.type:'video', src: this.backgroundArr[this.index].previewData?this.backgroundArr[this.index].previewData.src:this.src,poster: 'http://image.zuma.com/upload/976346440358859899.jpg'}
      this.$emit("titleChanged",this.index,obj,this.$refs.video)
    },
    resizedown(str){
      console.log("resizedown")
      let that = this
      this.dx = event.clientX;//当你第一次单击的时候，存储x轴的坐标。
      this.nowElPer = (parseFloat(this.items[this.index].width)/100).toFixed(4)
      console.log("this.items[this.index+1].width",this.items[this.index+1])
      console.log("this.items[this.index-1].width",this.items[this.index-1])
      this.nextElPer = this.items[this.index+1]?(parseFloat(this.items[this.index+1].width)/100).toFixed(4):0
      this.preElPer = this.items[this.index-1]?(parseFloat(this.items[this.index-1].width)/100).toFixed(4):0
      this.totalPer = parseFloat(this.nowElPer) + parseFloat(this.nextElPer)
      this.totalTwoWidth = parseFloat((this.totalWidth*this.totalPer).toFixed(4))
      this.nowElWidth = parseInt(this.nowElPer*this.totalWidth)
      this.nextElWidth = parseInt(this.nextElPer*this.totalWidth)
      this.preElWidth = parseInt(this.preElPer*this.totalWidth)

      document.onmousemove = function (event) {
          if(str.trim() == 'right'){
            that.dw = event.clientX - that.dx
            that.nowElWidth2 = that.dw + parseInt(that.nowElWidth)
            that.nextElWidth2 = parseInt(that.nextElWidth) - that.dw
            if (that.nowElWidth2 <= 50) {//当盒子缩小到一定范围内的时候(现在设置最小宽度为横条宽度的0.05倍(即5%))，让他保持一个固定值，不再继续改变
                that.nowElWidth2 = 50
                that.nextElWidth2 = (that.nowElWidth -50) + that.nextElWidth
            }
            if (that.nextElWidth2 <= 50) {
                that.nowElWidth2 = that.nowElWidth + (that.nextElWidth -50)
                that.nextElWidth2 = 50
            }
            that.nowElPer = that.nowElWidth2 /parseInt(that.totalWidth)
            that.nextElPer = that.nextElWidth2/parseInt(that.totalWidth)
            that.$emit("widthChange",str,that.index,that.nowElPer,that.nextElPer)
        }else if(str.trim() == 'left'){
            console.log(99987)
            that.dw = that.dx - event.clientX 
            that.nowElWidth2 = that.dw + parseInt(that.nowElWidth)
            that.preElWidth2 = parseInt(that.preElWidth) - that.dw

            if (that.nowElWidth2 <= 50) {//当盒子缩小到一定范围内的时候(现在设置最小宽度为横条宽度的0.05倍(即5%))，让他保持一个固定值，不再继续改变
                that.nowElWidth2 = 50
                that.preElWidth2 = (that.nowElWidth -50) + that.preElWidth
            }
            if (that.preElWidth2 <= 50) {
                that.nowElWidth2 = that.nowElWidth + (that.preElWidth -50)
                that.preElWidth2 = 50
            }
            that.nowElPer = that.nowElWidth2 /parseInt(that.totalWidth)
            that.preElPer = that.preElWidth2/parseInt(that.totalWidth)
            that.$emit("widthChange",str,that.index,that.nowElPer,that.preElPer)
        }

      }
      document.onmouseup = function (event) {
        document.onmousedown = null;
        document.onmousemove = null;
      }

    },
  },
  mounted:function(){
    this.$emit("videoT",this.$refs.video,this.index)
  }
 }
</script>
<style scoped>
  .zm-component-main{
      width: 1903px;
      height: 300px;
      display: inline-block;
  }
  .zm-component-combination{
    width: 100%;
    height: 100%;
  }
  .zm-component-banner{
    width: 100%;
    height: 100%;
    position:relative;
  }
  .nowIsSelect{
    border: 2px solid red;
    box-sizing: border-box;
  }
  .zm-bg-banner-PIC{
    width: 100%;
    height: 100%;
    background-size:cover; 
    background-position: center;
    background-repeat: no-repeat;
  }

  .zm-bg-banner-BGC{
    width: 100%;
    height: 100%;
  }

  .zm-bg-banner-Video{
    width: 100%;
    height: 100%;
    overflow:hidden
  }
  video{
    object-fit: cover;
    width:100%;
    height:100%;
  }
  .span-video{
    display: inline-block; 
    width: 30px; 
    height: 30px; 
    position: absolute; 
    left: 3px; 
    bottom: 2px; 
    color: rgb(255, 255, 255); 
    font-size: 28px;
    /* background-color:red */
  }
  .zm-bg-banner-vein{
    z-index:1;
    width: 100%; 
    height: 100%;

    left:0px;
    top:0px;
    position:absolute;
  }

  .zm-banner-resizeR,
  .zm-banner-resizeL {
  top: 10%;
  height: 20px;
  width: 18px;
  margin-top: -9px;
  background-color: #fff;
  position: absolute;
  cursor: w-resize;
  z-index: 2;
  transform: rotate(90deg);
  background: -webkit-gradient(linear, 0 0, 0 bottom, from(#fff), to(#e6e6e6));
  border-radius: 2px;
  border: 1px solid #e4e4e4;
  box-shadow: 0 0 5px rgba(218, 227, 233, 0.6);
  user-select: none;
  }
  .zm-banner-resizeR svg,
  .zm-banner-resizeL svg {
  width: 100%;
  height: 100%;
  stroke: #c9c9c9;
  fill: none;
  stroke-width: 12px;
  }
  .zm-banner-resizeR > svg:hover,
  .zm-banner-resizeL > svg:hover {
    fill: #3089d5;
  }
  .zm-banner-resizeR {
    right: -10px;
  }
  .zm-banner-resizeL {
    left: -10px;
  }

  .banner-Column-label {
    width: 130px;
    height: 28px;
    background-color: #3089d5;
    position: absolute;
    left: 0;
    top: -28px;
    border-top-left-radius: 5px;
    border-top-right-radius: 5px;
    color: #fff;
    text-align: center;
    line-height: 28px;
    font-size: 13px;
  }
</style>
