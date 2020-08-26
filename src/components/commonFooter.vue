<template>
  <footer>
    <div class="wrapper">
      <p>长路漫漫漫漫，快来一起学习吧！
<!--        <a href="#" target="_self"><i class="iconfont icon-github"></i></a>-->
<!--        <a href="#" target="_blank"><i class="iconfont icon-zhihu"></i></a>-->
      </p>
      <p>欢迎光临</p>
      <p>本站已经被访问 {{times}} 次！</p>
      <a class="beian" target="_blank" href="http://www.beian.miit.gov.cn/state/outPortal/loginPortal.action">渝ICP备19017793号</a>
    </div>
    <transition name="slide-fade">
      <div v-if="isTop"  class="toTop iconfont icon-top" @click="toTop"></div>
    </transition>
  </footer>
</template>
<script>
import {webUrl} from "../../static/js/public.js"

export default {
  props: {
    times: Number
  },
  data(){
    return{
      isTop:false,
      visit:486
    }
  },
  created(){
    window.addEventListener('scroll', this.scroll);
  },
  methods:{
    scroll:function(){
      let scroll=document.body.scrollTop || document.documentElement.scrollTop;
      if(scroll>100){
        this.isTop=true;
      }else{
        this.isTop=false;
      }
    },
    toTop:function(){
      //Math.animation = function (from, to, duration, easing, callback) {}
      Math.animation(document.documentElement.scrollTop, 0,800,'Quart.easeOut', function (value) {
          document.documentElement.scrollTop = value;
          document.body.scrollTop = value;
      });
    }
  }
}
</script>

<style lang="scss" scoped>
  .beian{
    display: block;
    font-size: 10px;
    margin-top: 8px;
  }
footer{
  background:#303133;
  color: #bbb;
  box-shadow: 0 -2px 4px 1px rgba(0, 0, 0, 0.5);
  padding: 10px 0;
  padding-bottom: 5px;
  text-align: center;
  .wrapper .iconfont{
    color: #bbb;
    font-size:20px;
    margin:0 5px;
    &:hover{
      color: #fff;
    }
  }
  .toTop{
    position: fixed;
    right: 20px;
    bottom: 150px;
    font-size: 40px;
    color: #666;
    cursor: pointer;
    z-index: 9999;
    &:hover{
      color: #333;
    }
  }
  .slide-fade-enter-active,.slide-fade-leave-active{
    transition: all .3s ease;
  }
  .slide-fade-enter, .slide-fade-leave-to{
    transform: translateY(10px);
    opacity: 0;
  }
}


@media (min-width: 768px) {//pc
  footer{
    padding: 15px 0;
    padding-bottom: 8px;
    .wrapper .iconfont{
      font-size:20px;
    }
    .toTop{
      position: fixed;
      // right: 50%;
      right: 10%;
      bottom: 50px;
      // margin-right: -690px;
      font-size: 40px;
      color: #666;
      cursor: pointer;
      z-index: 9999;
      &:hover{
        color: #999;
      }
    }
  }
}
</style>
