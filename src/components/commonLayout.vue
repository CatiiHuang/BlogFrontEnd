<template>
  <div class="body">
    <div class="bg">
      <div class="stars">
        <div
          class="star"
          ref="star"
          v-for="(item, index) in starsCount"
          :key="index"
        ></div>
      </div>
    </div>

    <div class="container">
      <common-header></common-header>

      <!-- 子路由 -->
      <div class="cont">
        <transition name="slide-fade">
          <!-- <keep-alive> -->
          <router-view></router-view>
          <!-- </keep-alive> -->
        </transition>
      </div>

      <Aplayer
        :music="musicList[0]"
        :list="musicList"
        class="aplayer"
        :shuffle="t"
        :showLrc="t"
        v-if="musicList.length >= 1"
        repeat="repeat-all"
        :listFolded="t"
        :float="t"
        theme="#9d9d9d"
        listMaxHeight="100"
        :howLrc="t"
        :autoplay="t"
        :mini="min"
      >
      </Aplayer>

      <div class="footer">
        <common-footer :times="times"></common-footer>
      </div>
    </div>
  </div>
</template>

<script>
import CommonHeader from "../components/commonHeader";
import CommonFooter from "../components/commonFooter";
import { checkSign } from "../../static/js/public.js";
import { webUrl } from "../../static/js/public.js";
import VueAplayer from "vue-aplayer";

export default {
  mixins: [checkSign],
  data() {
    return {
      starsCount: 800,
      distance: 800,
      items: [],
      times: 1,
      t: true,
      min: false,
      f: false,
      flag: false,
      musicList: []
    };
  },
  components: {
    CommonHeader,
    CommonFooter,
    Aplayer: VueAplayer
  },
  mounted() {
    let that = this;
    /*播放器*/
    this.upMusic().then(async res => {
      this.musicList = await res;
    });
    if (this._isMobile()) {
      this.min = true;
    }
    /*播放器结束*/
    this.$axios.get(webUrl + "visitied").then(res => {
      that.times = res.data.data;
    });
    let _this = this;
    // 原生js
    let _starList = document.getElementsByClassName("star");
    let starArr = Array.prototype.slice.call(_starList);
    // vue
    let starList = this.$refs.star;
    // 遍历添加样式
    starArr.forEach(item => {
      var s = 0.2 + Math.random() * 1;
      var thisDistance = _this.distance + Math.random() * 300;
      item.style.transformOrigin = `0 0 ${thisDistance}px`;
      item.style.transform = `translate3d(0,0,-${thisDistance}px) rotateY(${Math.random() *
        360}deg) rotateX(${Math.random() * -50}deg) scale(${s},${s})`;
    });
  },
  methods: {
    _isMobile() {
      let flag = navigator.userAgent.match(
        /(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i
      );
      return flag;
    },
    async upMusic() {
      var allList = [];
      var songList = [];
      this.$axios
        .get("https://www.temdu.com/api/Get.php", {
          params: {
            media: "netease",
            type: "collect",
            id: "2664677593",
            pass: "233333"
          }
        })
        .then(result => {
          allList = result.data;
          allList.forEach(item => {
            this.$axios("https://www.temdu.com/api/Get.php", {
              params: {
                media: "netease",
                type: "all",
                id: item.song_id,
                pass: "233333"
              }
            }).then(res => {
              var obj = {};
              obj.title = res.data.name;
              obj.src = res.data.url;
              obj.pic = res.data.cover;
              obj.lrc = res.data.lrc;
              obj.artist = res.data.author;
              songList.push(obj);
            });
          });
        }); //请求完
      return songList;
    }
  }
};
</script>

<style lang="css" scoped>
.body {
  background: radial-gradient(
    200% 100% at bottom center,
    #f7f7b6,
    #e96f92,
    #75517d,
    #1b2947
  );
  background: radial-gradient(
    220% 105% at top center,
    #1b2947 10%,
    #75517d 40%,
    #e96f92 65%,
    #f7f7b6
  );
  background-attachment: fixed;
  overflow: hidden;
  min-height: 100vh;
}

@keyframes rotate {
  0% {
    transform: perspective(350px) rotateZ(20deg) rotateX(-40deg) rotateY(0);
  }
  100% {
    transform: perspective(350px) rotateZ(20deg) rotateX(-40deg)
      rotateY(-360deg);
  }
}
.bg {
}
.stars {
  transform: perspective(600px);
  transform-style: preserve-3d;
  position: fixed;
  bottom: 0;

  perspective-origin: 40% 100%;
  left: 60%;
  animation: rotate 90s infinite linear;
}

.star {
  width: 2px;
  height: 2px;
  background: #f7f7b6;
  position: absolute;
  top: 0;
  left: 0;
  backface-visibility: hidden;
}
.container {
  width: 100%;
  margin: 0 auto;
  padding-bottom: 120px;

  box-sizing: border-box;
  min-height: 100%;
}

.content {
  margin-bottom: 1.8rem;
}
.cont-ul {
  padding-top: 0.5rem;
  background-color: #ccc;
}
.cont-ul::after {
  content: "";
  display: block;
  clear: both;
  width: 0;
  height: 0;
}
.slide-fade-enter-active {
  transition: all 0.3s ease;
}
.slide-fade-enter,
.slide-fade-leave-to {
  transform: translateY(10px);
  opacity: 0;
}
.footer {
  position: absolute;
  bottom: 0;
  width: 100%;
  opacity: 0.8;
}
.cont {
  opacity: 0.9;
  z-index: 5;
}
.aplayer {
  position: fixed;
  right: 20px;
  bottom: 100px;
}
@media screen and (min-width: 1000px) {
  .aplayer {
    width: 500px !important;
  }
}
.aplayer {
  margin: 0;
}
</style>
