<template>
  <div @touchmove.prevent.stop="touchmove" @touchend="touchend" @touchstart="touchstart">
    <div class="header"></div>
    <transition :name="transitionName">
      <router-view ref="router" class="child-view"></router-view>
    </transition>
  </div>
</template>

<script>
/* eslint-disable no-new */
export default {
  data() {
    return {
      transitionName: "slide-left",
      initPageX:0,
      initPageY:0
    };
  },
  mounted(){
    // console.dir(this.$refs.router)
  },
  methods: {
    touchstart(e) {
      let touch = e.touches;
      console.log("start");
      // debugger;
      this.initPageX = touch[0].pageX;
      this.initPageY = touch[0].pageY;
    },
    touchmove(e) {
      //console.log("move");
      //console.log(e);
      let touch = e.changedTouches[0];
      let deltaX = touch.pageX - this.initPageX;
      let deltaY = touch.pageY - this.initPageY;
     // debugger
      if(deltaX>0){
        this.transitionName = "slide-right";
      }else{
        this.transitionName = "slide-left";
      }
      // 判断横向还是纵向滑动
      if (Math.abs(deltaX) > Math.abs(deltaY)&&Math.abs(deltaX)>window.innerWidth/2&&this.$route.path!=='/') {
       // debugger
        this.$children[0]&&this.$children[0].$router.goBack();
      }
    },
    touchend(e) {
      let touch = e.changedTouches[0];
      // // let deltaX=touch.pageX-this.initPageX;
      // // let deltaY=touch.pageY-this.initPageY;
      // this.initPageX = touch.pageX;
      // this.initPageY = touch.pageY;
      console.log(touch);
      console.log("end");
      // console.log(e);
    }
  },
  beforeRouteUpdate(to, from, next) {
    console.log(to.path)
    let isBack = this.$router.isBack;
    // if (isBack) {
    //   this.transitionName = "slide-right";
    // } else {
    //   this.transitionName = "slide-left";
    // }
    this.$router.isBack = false;
    setTimeout(() => {
      next();
    }, 60);
  }
};
</script>

<style scoped>
.child-view {
  position: absolute;
  transition: all 0.4s cubic-bezier(0.55, 0, 0.1, 1);
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background: #fff;
  will-change: transform;
}
.slide-left-enter,
.slide-right-leave-active {
  -webkit-transform: translateX(100%);
  transform: translateX(100%);
  z-index: 1;
}
.slide-left-leave-active,
.slide-right-enter {
  -webkit-transform: translateX(-50px);
  transform: translateX(-50px);
  z-index: -1;
}
.header {
  position: absolute;
  height: 44px;
  width: 100%;
}
</style>
