<template>
  <div v-if="imgshow" @click="imgShowClick" class="bigimg-wrop">
    <div class="bigimg-wrop-scroll">
      <img class="touchImg" ref="image" :style="{transitionDuration:tranDuration,transform:`scale(${touchScale}) translate(${touchTranslate.x}px,${touchTranslate.y}px)`}" @touchend="touchend" @touchstart="touchstart" @touchmove="touchmove"
        :src="src">
    </div>
  </div>
</template>
<script>
  export default {
    model :{
      prop: 'imgshow',
      event: 'change'
    },
    props: {
      src:{
        type: String,
        default:''
      },
      maxScale: {
        type: Number,
        default: 3
      },
      minScale: {
        type: Number,
        default: 1
      },
      imgshow:{
        type:Boolean,
        default: false
      }
    },
    data() {
      return {
        startDistance:0,//双指初始距离长度
        touchScale:1,//放大的倍率
        lastTouchScale:1,//上次放大的倍率
        touchTranslate:{x:0,y:0},//移动坐标
        startTranslate:{x:0,y:0},//初始坐标
        lastTranslat:{x:0,y:0},//上次移动过的坐标
        canMove:false,
        tranDuration:'0',
      }
    },
   computed: {
  
   },
    methods: {
      imgShowClick(){
        this.$emit('change', false)
        this.startDistance=0;
        this.touchScale=1;
        this.lastTouchScale=1;
        this.touchTranslate={x:0,y:0};
        this.startTranslate={x:0,y:0};
        this.lastTranslat={x:0,y:0};
      },
      touchstart(e) {
         this.tranDuration = '0s';
         this.ceshi = '开始';
        if(e.touches.length===1){
              this.startTranslate.x = e.touches[0].pageX;
              this.startTranslate.y =e.touches[0].pageY;
          }
        if (e.touches.length === 2) {
          this.startDistance = this.getDistance(e.touches[0], e.touches[1]);  
        }
        this.canMove = true;
        
      },
      touchmove(e) {
        if(e.touches.length === 1&&this.canMove){
            let transX = e.touches[0].pageX-this.startTranslate.x;
            let transY = e.touches[0].pageY-this.startTranslate.y;
            this.touchTranslate = {x:transX/this.touchScale+this.lastTranslat.x,y:transY/this.touchScale+this.lastTranslat.y};
        }
        if (e.touches.length === 2&&this.canMove) {
          this.touchScale = (this.getDistance(e.touches[0], e.touches[1]) / this.startDistance) * this.lastTouchScale;
          if (this.touchScale >= this.maxScale) this.touchScale = this.maxScale;
        }
      },
      touchend(e) {
        this.ceshi = '结束';
        this.canMove = false;
        this.lastTouchScale = this.touchScale;
        this.lastTranslat = this.touchTranslate;
        this.tranDuration = '0.5s';
        let imgOverflowHeight = Math.abs(this.$refs.image.height*this.lastTouchScale-window.innerHeight)/2/this.lastTouchScale;//除以倍率是因为坑爹的Translate的移动距离也会随着倍率增大而减少
        let imgOverflowWidth = Math.abs(this.$refs.image.width*this.lastTouchScale-window.innerWidth)/2/this.lastTouchScale;
        if(this.touchTranslate.x>imgOverflowWidth)this.touchTranslate.x =imgOverflowWidth;
        else if(this.touchTranslate.x<-(imgOverflowWidth))this.touchTranslate.x =-(imgOverflowWidth);
        if(this.touchTranslate.y>imgOverflowHeight)this.touchTranslate.y =imgOverflowHeight;
        else if(this.touchTranslate.y<-(imgOverflowHeight))this.touchTranslate.y =-(imgOverflowHeight);
        
        if (this.touchScale <= this.minScale) this.touchScale = this.minScale;
    
       
      },
      getDistance(touch1, touch2) {//勾股定理获得双指放大距离
        let x = touch2.pageX - touch1.pageX,
          y = touch2.pageY - touch1.pageY;
        return Math.sqrt((x * x) + (y * y));
      }
    },
    created() {
      
    },
    mounted() {
      
    },
    watch: {}
  }

</script>
<style>
  .touchImg {
    width: auto;
    height: auto;
    max-width: 100%;
    max-height: 100%;
  }
  .bigimg-wrop-scroll {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
    flex-direction: column;
    overflow: hidden;
  }

  .bigimg-wrop {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: #000;
    z-index: 1000;
  }

</style>
