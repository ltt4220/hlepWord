<template>
    <div :id="id" :uid="uid" :class="{nowIsSelect:classShow}" class="zm-component-main" :style="{position: 'absolute', width: style.width+'px',height: style.height+'px',left: style.left+'px',top: style.top+'px',zIndex: style.zIndex,display: style.display}" @mousedown.stop.prevent.left="mousedown($event),other($event)"  @contextmenu.stop.prevent="openMenu">
      <div  v-for="(item,index) in items" :key="index" :style="{display:display,width:items[index].width,height:height}">
          <div :is="item.component" :index="index" :dataList="nowLength" :widthList='widthList' v-on:titleChanged = "zidingyi" @widthChange='widthChange' :items='items' :totalWidth='style.width' :backgroundArr='backgroundArr' @videoT="videoT"></div>
      </div>
    <div @click.stop = "bannerMainClick" v-show="mainBannerVeinShow" refs="mainBannerVein" class="zm-main-banner-vein"></div>
  </div>
</template>
<script>
import v from './CombinationBar/comCombinationBannerVideo'
import i from './CombinationBar/comCombinationBannerPic'
import c from './CombinationBar/comCombinationBannerColor'
export default {
  name: 'vic',
  data: function () {
    return {
      allVue:[],//存储所有组件实例
      isbanner:false,//判断是否点击过组合横条
      mainBannerVeinShow:true,//是否显示最外层遮罩层
      classShow:false,//
      isCombinationBanner: true,//
      initClick:0,
      currentIndex:0,
      backgroundArr:[
        {
          isVideoPlay: true,
          isLoopPlay: true,
          isActive: false,
          playSpeed: '正常播放',
          nowIndex: 0,
          tabIndex: 0,
          textureIndex: 0,
          depthIndex: 0,
          historyList: { list: [{type: 'video', title: '横条视频', src: 'http://image.zuma.com/upload/968828205866584890.mp4', poster: 'http://image.zuma.com/upload/976346440358859899.jpg'}], title: '使用过的背景', type: 'img' },
          initList: null,
          previewData: {type: 'video', src: 'http://image.zuma.com/upload/968828205866584890.mp4', poster: 'http://image.zuma.com/upload/976346440358859899.jpg'},
          nowVideo: null,
          background: {
            val: 0,
            childLeft: 0,
            childWidth: 10
          },
          coverage: {
            val: 0,
            childLeft: 0,
            childWidth: 10,
            color: 'rgba(255,255,255,0)'
          },
          bottom: {
            val: 0,
            childLeft: 0,
            childWidth: 10,
            color: 'rgba(255,255,255,0)'
          }
        },
        {
          isVideoPlay: true,
          isLoopPlay: true,
          isActive: false,
          playSpeed: '正常播放',
          nowIndex: 0,
          tabIndex: 0,
          textureIndex: 0,
          depthIndex: 0,
          historyList: { list: [], type: 'img' },
          initList: null,
          previewData: {type: 'img', poster: 'http://image.zuma.com/upload/976346440358859899.jpg'},
          nowVideo: null,
          background: {
            val: 0,
            childLeft: 0,
            childWidth: 10
          },
          coverage: {
            val: 0,
            childLeft: 0,
            childWidth: 10,
            color: 'rgba(255,255,255,0)'
          },
          bottom: {
            val: 0,
            childLeft: 0,
            childWidth: 10,
            color: 'rgba(255,255,255,0)'
          }
        },
        {
          isVideoPlay: true,
          isLoopPlay: true,
          isActive: false,
          playSpeed: '正常播放',
          nowIndex: 0,
          tabIndex: 0,
          textureIndex: 0,
          depthIndex: 0,
          historyList: { list: [], type: 'img' },
          initList: null,
          previewData: {type: 'color', color: 'rgba(227, 102, 62,0.5)'},
          nowVideo: null,
          background: {
            val: 50,
            childLeft: 80*0.36,
            childWidth: 10
          },
          coverage: {
            val: 0,
            childLeft: 0,
            childWidth: 10,
            color: 'rgba(255,255,255,0)'
          },
          bottom: {
            val: 0,
            childLeft: 0,
            childWidth: 10,
            color: 'rgba(255,255,255,0)'
          }
        },
      ],
      items:[{
        type:'video',
        resizeLShow:false,
        resizeRShow:false,
        ColumnLabelShow:false,
        width:'33%',
        previewData:null,
        vienImg:null,
        textureIndex:0,
        coverage:'rgba(255,255,255,0)',
        bottom:'rgba(255,255,255,0)',
        nowIsSelect:false,
        depthIndex:0,
        component: v
      },{
        type:'img',
        resizeLShow:false,
        resizeRShow:false,
        ColumnLabelShow:false,
        width:'34%',
        previewData:null,
        vienImg:null,
        textureIndex:0,
        coverage:'rgba(255,255,255,0)',
        bottom:'rgba(255,255,255,0)',
        nowIsSelect:false,
        depthIndex:0,
        component: i
      },{
        type:'color',
        resizeLShow:false,
        resizeRShow:false,
        ColumnLabelShow:false,
        width:'33%',
        previewData:null,
        vienImg:null,
        coverage:'rgba(255,255,255,0)',
        bottom:'rgba(255,255,255,0)',
        nowIsSelect:false,
        depthIndex:0,
        component: c
      }],
      widthList:[],
      id: '',
      uid: '',
      inrowlayer: '',
      style: {
          width: 1893,
          height: 300,
          left: 0,
          top: 0,
          zIndex: 0,
          rotate: 0,
          display:'flex',
      },
      display:'inline-block',
      height:'100%',
      pattern: [1,1,1,1,1,1,1,1],
      isShowRotateBtn: false,
      buttonList:[
        {type:'notFullPage',title:"取消全屏",icon:'icon-puman',callback:this.fullOrNotPage},
        {type:'style',title:'样式切换',icon:'icon-style'},
        {type:'del',title:'删除',icon:'icon-delete',callback:this.deleteBanner}
      ],
      backgroundData:{
        isVideoPlay: true,
        isLoopPlay: true,
        isActive: false,
        playSpeed: '正常播放',
        nowIndex: 0,
        tabIndex: 0,
        textureIndex: 0,
        depthIndex: 0,
        historyList: { list: [], type: 'img' },
        initList: null,
        previewData: null,
        nowVideo: null,
        background: {
          val: 0,
          childLeft: 0,
          childWidth: 10
        },
        coverage: {
          val: 0,
          childLeft: 0,
          childWidth: 10,
          color: 'rgba(255,255,255,0)'
        },
        bottom: {
          val: 0,
          childLeft: 0,
          childWidth: 10,
          color: 'rgba(255,255,255,0)'
        }
      },
    }
  },
  watch:{//观察子组件数据的变化，变化则执行相关事件
    backgroundArr: {//backgroundData监测整个对象的变化，'backgroundData.previewData'监测对象中某个属性的变化（需要加引号）
　　　　handler(newValue, oldValue) {
         this.items[this.currentIndex].type = this.backgroundArr[this.currentIndex].previewData.type
        //  this.items[this.currentIndex].previewData = this.backgroundArr[this.currentIndex].previewData//这种会发生引用同一个地址，就会产生改变一一个横条列时，其他列也会跟着改变
         this.items[this.currentIndex].previewData = Object.assign({},this.backgroundArr[this.currentIndex].previewData)
         this.items[this.currentIndex].vienImg = this.texturePicture
         this.items[this.currentIndex].depthIndex = this.backgroundArr[this.currentIndex].depthIndex
         this.items[this.currentIndex].textureIndex = this.backgroundArr[this.currentIndex].textureIndex
         this.items[this.currentIndex].coverage = this.backgroundArr[this.currentIndex].coverage.color
        //  this.items[this.currentIndex].coverage = (Object.assign({},this.backgroundArr[this.currentIndex].coverage)).color
         this.items[this.currentIndex].bottom = this.backgroundArr[this.currentIndex].bottom.color
        //  this.items[this.currentIndex].bottom = (Object.assign({},this.backgroundArr[this.currentIndex].bottom)).color
　　　　},
　　　　deep: true
　　}
  },
  computed:{
    nowLength(){//计算子组件总个数
      return this.items.length
    },
    widthListCl(){//初始化列宽数组
      let width = (1/this.nowLength*100)+"%"
      this.widthList = []
      for(let i = 0;i < this.nowLength;i++){
        this.widthList.push(width)
        // this.items[i].width = 1/this.nowLength
        this.items[i].width = width
      }
      // return this.widthList
    },
    nowWidth(){//获取初始化列宽
      return (1/this.nowLength*100)+"%"
    },
    texturePicture(){
        let url = '';
        if(this.backgroundArr[this.currentIndex].depthIndex === 0){
          url = 'url(http://local.zuma.com:8888/background/light_0' + this.backgroundArr[this.currentIndex].textureIndex + '.png)'
        } else{
          url = 'url(http://local.zuma.com:8888/background/dark_0' + this.backgroundArr[this.currentIndex].textureIndex + '.png)'
        }
        return this.backgroundArr[this.currentIndex].textureIndex==0 ? '' : url;
    } 
  },
  methods:{
    videoT:function(video,index){//通过$emit子组件点击事件来改变父组件的相关数据
      this.backgroundArr[this.initClick?this.initClick:index].nowVideo = video;
    },
    zidingyi(index,obj,video){//通过$emit子组件点击事件来改变父组件的相关数据
      console.log("zmEditor.$store.state.component.selectList[0]========",zmEditor.$store.state.component.selectList[0])
      this.initClick = index;
      this.currentIndex = index;
      this.backgroundArr[this.currentIndex].nowVideo = video;

      if(index == 0){
        this.items.forEach((element,i) => {
          if(i==index){
            this.items[index].resizeLShow = false
            this.items[index].resizeRShow = true
            this.items[index].ColumnLabelShow = true
            this.items[index].nowIsSelect = true
          }else{
            this.items[i].resizeLShow = false
            this.items[i].resizeRShow = false
            this.items[i].ColumnLabelShow = false
            this.items[i].nowIsSelect = false
          }
        });
      }else if(index == (this.items.length-1)){
        this.items.forEach((element,i) => {
          if(i==index){
            this.items[index].resizeLShow = true
            this.items[index].resizeRShow = false
            this.items[index].ColumnLabelShow = true
            this.items[index].nowIsSelect = true
          }else{
            this.items[i].resizeLShow = false
            this.items[i].resizeRShow = false
            this.items[i].ColumnLabelShow = false
            this.items[i].nowIsSelect = false
          }
        });
      }else if(index != 0 || index != (this.items.length-1)){

        this.items.forEach((element,i) => {
          if(i==index){
            this.items[index].resizeLShow = true
            this.items[index].resizeRShow = true
            this.items[index].ColumnLabelShow = true
            this.items[index].nowIsSelect = true
          }else{
            this.items[i].resizeLShow = false
            this.items[i].resizeRShow = false
            this.items[i].ColumnLabelShow = false
            this.items[i].nowIsSelect = false
          }
        });
      }
      zmEditor.$children[0].isCtrlShow = false; //编辑框隐藏;
      this.classShow = false
      this.btnList()
      this.isbanner = true
      
    },
    widthChange(str,index,nowPer,otherPer){
       setTimeout(() => {
          this.items[index].width = Math.ceil(nowPer*100) + '%'
            if(str.trim() == 'right'){
              if(this.items.length >= 2){
                this.items[index+1].width = otherPer*100 + '%'
              }
            }else if(str.trim() == 'left'){
              if(this.items.length >= 2){
                this.items[index-1].width = otherPer*100 + '%'
              }
          }
       },50)
    },
    // other(e){
    //   zmEditor.$children[0].isCtrlShow = true; //编辑框显示;
    // },
    cl2() {//编辑和设置按钮点击事件
      event.currentTarget.getAttribute("title").trim() == "编辑" ? this.bannerTitle = "横条背景" : this.bannerTitle = "横条设置"
      event.currentTarget.getAttribute("title").trim() == "编辑" ? this.nowIndex = 0 : this.nowIndex = 1;
      zmEditor.$store.commit('setBackgroundData', {key: 'comType', value: 'banner'});
      zmEditor.$store.commit('changePaneData', {key: 'paneType', value: true});
      zmEditor.$store.commit('changePaneData', {key: 'paneTitle', value: this.bannerTitle});

      zmEditor.$store.commit('changePaneData', {key: 'paneMode', value: "/editor/pane/backgroundPreview.vue"});
      zmEditor.$store.commit('setBackgroundData', {key: 'nowShowStyle', value: 'img'});
      // this.backgroundData.tabIndex = this.nowIndex;
      this.backgroundArr[this.currentIndex].tabIndex = this.nowIndex;
    },
    fullOrNotPage:function(){//全屏，取消全屏事件
        let FullPageObj = {type:'fullPage',title:"全屏",icon:'icon-quanping1',callback:this.fullOrNotPage}
        let notFullPageObj = {type:'notFullPage',title:"取消全屏",icon:'icon-puman',callback:this.fullOrNotPage}
        if(this.buttonList[0].type === "notFullPage"){
            this.style.width = 1200;
            this.style.left = 0
            this.buttonList.splice(0,1,FullPageObj)
        }else{
            this.style.width = 1893;
            this.style.left = -345
            this.buttonList.splice(0,1,notFullPageObj)
        }
    },
    bannerMainClick(){//父组件点击事件
      console.log(" window.screen.width", window.screen.width)
      let component = zmEditor.$store.state.component
      this.allVue = this.allVue.concat(component.bodyRowList,component.footRowList,component.headRowList,component.pasteList)
      this.allVue.forEach((ele,i)=>{
        if(ele == zmEditor.$store.state.component.selectList[0]){
        }else{
          if(ele.isCombinationBanner){
            ele.classShow = false
            ele.mainBannerVeinShow = true
            if(ele.items){
              ele.items.forEach((elememt,index)=>{
                  elememt.resizeLShow= false
                  elememt.resizeRShow = false
                  elememt.ColumnLabelShow = false
                  elememt.nowIsSelect = false
              })
            }
          }

        }
      })
      let nowCom = zmEditor.$store.state.component.selectList[0];
      zmEditor.$store.state.background.oldCombination = nowCom;
      console.log("zmEditor.$store.state.background.oldCombination---",zmEditor.$store.state.background.oldCombination)
      this.mainBannerVeinShow = !this.mainBannerVeinShow
      this.classShow = !this.classShow
      this.classShow = true
      this.isbanner = true

      let FullPageObj = {type:'fullPage',title:"全屏",icon:'icon-quanping1',callback:this.fullOrNotPage}
      let notFullPageObj = {type:'notFullPage',title:"取消全屏",icon:'icon-puman',callback:this.fullOrNotPage}
      if(this.style.width == 1893){
          this.buttonList.splice(0,1,notFullPageObj)
      }else{
          this.buttonList.splice(0,1,FullPageObj)
      }
    },
    rightAdd(){//右侧添加列
      let obj = Object.assign({},this.items[this.initClick])
      obj.resizeLShow = false
      obj.resizeRShow = false
      obj.ColumnLabelShow = false
      obj.nowIsSelect = false
      this.items.splice(this.currentIndex,0,Object.assign({},obj));
      // this.backgroundArr.splice(this.currentIndex,0,this.backgroundData);
      // this.backgroundArr.splice(this.currentIndex,0,Object.assign({},this.backgroundArr[this.initClick]));
      
      //深层拷贝对象（对象里面嵌套对象）,如果只是一般的对象（对象里没有嵌套对象）就用Object.assign({},obj)
      function deepClone(original, target){
        var target = target || {};// 如果target为undefined或没传参，设置空对象
        for(var prop in original){// 遍历原对象
          if(original.hasOwnProperty(prop)){// 只拷贝对象内部，不考虑原型链
            if(typeof original[prop] === 'object'){// 引用值
              if(Object.prototype.toString.call(original[prop]) === '[object Array]'){
                target[prop] = [];// 处理数组引用值
              }else{
                target[prop] = {};// 处理对象引用值
              }// 可以用三目运算符
              deepClone(original[prop],target[prop]);// 递归克隆
            }else{// 基本值
              target[prop] = original[prop];
            }  
          }
        }
        return target;
      }
      // var newSetObj = deepClone(this.backgroundArr[this.initClick]);
      var newSetObj = JSON.parse(JSON.stringify(this.backgroundArr[this.initClick])); //深层拷贝对象（对象里面嵌套对象）,如果只是一般的对象（对象里没有嵌套对象）就用Object.assign({},obj)
      this.backgroundArr.splice(this.currentIndex,0,newSetObj);
      
      this.items.forEach((element,i,)=>{
          if(this.initClick == i) {
            this.items[this.initClick].resizeLShow = true
            this.items[this.initClick].resizeRShow = true
            this.items[this.initClick].ColumnLabelShow = true
            this.items[this.initClick].nowIsSelect = true
          }else{
            this.items[i].resizeLShow = false
            this.items[i].resizeRShow = false
            this.items[i].ColumnLabelShow = false
            this.items[i].nowIsSelect = false
          }
            
      })
      let width = (1/this.items.length*100)+"%"
      for(let i = 0;i < this.items.length;i++){
        this.items[i].width = width
      }
      this.btnList()
    },
    deleteBanner(){//删除列
      this.items.forEach((element,i,)=>{
            if(element.nowIsSelect) {
                this.currentIndex = i;
            }
      })
      // if(this.currentIndex == 0) return
      this.items.splice(this.currentIndex,1);
      this.backgroundArr.splice(this.currentIndex,1);
      
      if(this.currentIndex == 0){
        this.items[this.currentIndex].nowIsSelect = true
      }else{
        this.items[this.currentIndex-1].nowIsSelect = true
      }
      this.currentIndex = this.currentIndex-1
      let [obj1,obj2,obj3,obj4,obj5,obj6,obj7] = [
          {type:'editor',title:"编辑",icon:'icon-110',callback:this.cl2},
          {type:'bannerSet',title:'设置',icon:'icon-shezhi2',callback:this.cl2},
          {type:'rightAdd',title:'右侧添加列',icon:'icon-icon',callback:this.rightAdd},
          {type:'leftMove',title:'左移',icon:'icon-zuoyi',callback:this.leftMove},
          {type:'rightMove',title:'右移',icon:'icon-youyi',callback:this.rightMove},
          {type:'delBanner',title:'删除',icon:'icon-shanchu2',callback:this.deleteBanner},
          {type:'del',title:'删除',icon:'icon-delete'},
      ]
      this.items.forEach((element,i,)=>{
          if(element.nowIsSelect) {
              this.currentIndex = i;
              this.initClick = i;
          }
      })
      let width = (1/this.items.length*100)+"%"
      for(let i = 0;i < this.items.length;i++){
        this.items[i].width = width
      }
      this.btnList()
    },
    leftMove(){//列左移
      this.items.forEach((element,i,)=>{
          if(element.nowIsSelect) {
              this.currentIndex = i;
          }
      })
      if(this.currentIndex == 0) return

      var nowEle = this.items[this.currentIndex]
      var preEle = this.items[this.currentIndex-1]

      var nowEleSet = this.backgroundArr[this.currentIndex]
      var preEleSet = this.backgroundArr[this.currentIndex-1]
      //当前被选中的列的前一列位置被后一列替换
      this.items.splice(this.currentIndex-1,1,nowEle)
      this.backgroundArr.splice(this.currentIndex-1,1,Object.assign({},nowEleSet))

      //当前被选中的列的位置被前一列替换
      this.items.splice(this.currentIndex,1,preEle)
      this.backgroundArr.splice(this.currentIndex,1,Object.assign({},preEleSet))

      this.items.forEach((element,i,)=>{
          if(element.nowIsSelect) {
              this.currentIndex = i;
              this.initClick = i;
          }
      })
      this.btnList()

    },
    rightMove(){//列右移
      this.items.forEach((element,i,)=>{
            if(element.nowIsSelect) {
                this.currentIndex = i;
            }
      })
      if(this.currentIndex == (this.items.length - 1)) return
      let nowEle = this.items[this.currentIndex]
      let nextEle = this.items[this.currentIndex+1]

      let nowEleSet = this.backgroundArr[this.currentIndex]
      let nextEleSet = this.backgroundArr[this.currentIndex+1]


      //当前被选中的列的下一列位置被上一列替换
      this.items.splice(this.currentIndex+1,1,Object.assign({},nowEle))
      this.backgroundArr.splice(this.currentIndex+1,1,Object.assign({},nowEleSet))
      // //当前被选中的列的位置被下一列替换
      this.items.splice(this.currentIndex,1,Object.assign({},nextEle))
      this.backgroundArr.splice(this.currentIndex,1,Object.assign({},nextEleSet))

      this.items.forEach((element,i,)=>{
          if(element.nowIsSelect) {
              this.currentIndex = i;
              this.initClick = i;
          }
      })
      this.btnList()
    },
    btnList(){//根据点击列判断设置按钮的组合
      let [obj1,obj2,obj3,obj4,obj5,obj6,obj7] = [
        {type:'editor',title:"编辑",icon:'icon-110',callback:this.cl2},
        {type:'bannerSet',title:'设置',icon:'icon-shezhi2',callback:this.cl2},
        {type:'rightAdd',title:'右侧添加列',icon:'icon-icon',callback:this.rightAdd},
        {type:'leftMove',title:'左移',icon:'icon-zuoyi',callback:this.leftMove},
        {type:'rightMove',title:'右移',icon:'icon-youyi',callback:this.rightMove},
        {type:'delBanner',title:'删除',icon:'icon-shanchu2',callback:this.deleteBanner},
        {type:'del',title:'删除',icon:'icon-delete'},
      ]
      this.items.forEach((element,i,)=>{
        if(element.nowIsSelect) {
            if(i == 0){
              this.buttonList = [obj1,obj2,obj3,obj5,obj6]
              this.items[i].resizeLShow = false
              if(this.items.length == 1){
                element.resizeLShow = false
                element.resizeRShow = false
                element.ColumnLabelShow = false
                this.buttonList = [obj1,obj2,obj3,obj7]
              }else{
                element.resizeLShow = false
                element.resizeRShow = true
                element.ColumnLabelShow = true
              }
            }else if(i == (this.items.length-1)){
              this.buttonList = [obj1,obj2,obj3,obj4,obj6]
              if(this.items.length == 1){
                element.resizeLShow = false
                element.resizeRShow = false
                element.ColumnLabelShow = false
                this.buttonList = [obj1,obj2,obj3,obj7]
              }else{
                element.resizeLShow = true
                element.resizeRShow = false
                element.ColumnLabelShow = true
              }
            }else{
              this.buttonList = [obj1,obj2,obj3,obj4,obj5,obj6]
              if(i != (this.items.length - 1) || i != 0){
                element.resizeLShow = true
                element.resizeRShow = true
                element.ColumnLabelShow = true
              }
            }
        }
      })
    }
  },
  beforeMount(){
    zmEditor.$store.state.background.oldCombination = null
  },
  mounted(){
    let that = this
    //当前被编辑框以外的点击事件
    $(document).on("click", function (event) {
        event.stopPropagation();
        if(that.isbanner == false) {
          return
        }
        if($(event.target).hasClass("zm-page-bodyColor") || $(event.target).hasClass("zm-row-container") || (zmEditor.$store.state.background.oldCombination != zmEditor.$store.state.component.selectList[0])){

        console.log("event.target--这是定义在组合横条中的全局点击事件如有影响请告知",event.target)
        zmEditor.$children[0].isCtrlShow = false //编辑框隐藏;
        if(zmEditor.$store.state.background.oldCombination != zmEditor.$store.state.component.selectList[0]){
          zmEditor.$children[0].isCtrlShow = true //编辑框隐藏;
        }
        
        that.mainBannerVeinShow = true
        that.classShow = false;
        
        that.buttonList = [
          {type:'notFullPage',title:"取消全屏",icon:'icon-puman',callback:that.fullOrNotPage},
          {type:'style',title:'样式切换',icon:'icon-style'},
          {type:'del',title:'删除',icon:'icon-delete',callback:that.deleteBanner}
        ]
        that.items.forEach((element,i)=>{
            element.resizeLShow = false
            element.resizeRShow = false
            element.ColumnLabelShow = false
            element.nowIsSelect = false
        })
        that.isbanner = false
        }else{
          console.log(543553)
          that.btnList()
          that.isbanner = true
        }
    })
  },
}
</script>
<style scoped>
 .zm-main-banner-vein{
    z-index:1;
    width: 100%;
    height: 100%;
    left:0px;
    top:0px;
    position:absolute;
  }
  .nowIsSelect{
    border: 2px solid red;
    box-sizing: border-box;
  }
</style>
