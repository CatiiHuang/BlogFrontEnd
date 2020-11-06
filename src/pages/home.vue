<template>
  <div class="page">
    <div class="wrapper">
      <el-row>
        <el-col :xs="24" :md="18" :lg="18" :xl="18">
          <div class="main">
            <ListHome v-loading="loading" :items="items"></ListHome>
            <div class="bottom-text">到底啦~</div>
          </div>
        </el-col>
        <el-col :xs="0" :md="6" :lg="6" :xl="6">
          <div class="aside">
            <div class="card">
              <p class="title">ABOUT ME</p>
              <img class="pic" src="../../static/img/p2.jpg" alt="" />
              <div class="row">
                <p>爱你所爱 ，行你所行 。</p>
                <div class="icons">
                  <a href="#" target="_self"
                    ><i class="iconfont icon-github"></i
                  ></a>
                  <a href="#" target="_self"
                    ><i class="iconfont icon-zhihu"></i
                  ></a>
                  <a
                    href="https://music.163.com/#/user/home?id=1343677472"
                    target="_target"
                    ><i class="iconfont icon-yinle"></i
                  ></a>
                  <a href="https://blog.csdn.net/qq_44375977" target="_target"
                    ><i class="iconfont icon-CN_csdnnet"></i
                  ></a>
                </div>
              </div>
            </div>
            <div class="card">
              <p class="title"></p>
              <div class="row">
                <!-- <a class="link" href="#" target="_blank">哈维</a> -->
              </div>
            </div>
          </div>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
import ListHome from "../components/list_home";
import { webUrl } from "../../static/js/public.js";
export default {
  data() {
    return {
      items: [],
      page: 0,
      total: 0,
      loading: false,
      pageSize: 10
    };
  },
  components: {
    ListHome
  },
  created() {
    this.getList();
    this.test();
    const self = this;
  },
  methods: {
    test() {
      const that = this;
      let jieLiu = (func, delay = 700) => {
        let timer = null;
        return function() {
          if (!timer) {
            func.call(this);
            timer = setTimeout(() => {
              timer = null;
            }, delay);
          }
        };
      };
      var height = 0;
      var index = 1;
      var cal = 1;

      //获取滚动条上下滑动的距离
      function getScrollTop() {
        var scrollTop = 0,
          bodyScrollTop = 0,
          documentScrollTop = 0;
        if (document.body) {
          bodyScrollTop = document.body.scrollTop;
        }
        if (document.documentElement) {
          documentScrollTop = document.documentElement.scrollTop;
        }
        scrollTop =
          bodyScrollTop - documentScrollTop > 0
            ? bodyScrollTop
            : documentScrollTop;
        return scrollTop;
      }

      //页面的总高度
      function getScrollHeight() {
        var scrollHeight = 0,
          bodyScrollHeight = 0,
          documentScrollHeight = 0;
        if (document.body) {
          bodyScrollHeight = document.body.scrollHeight;
        }
        if (document.documentElement) {
          documentScrollHeight = document.documentElement.scrollHeight;
        }
        scrollHeight =
          bodyScrollHeight - documentScrollHeight > 0
            ? bodyScrollHeight
            : documentScrollHeight;
        return scrollHeight;
      }

      //浏览器显示窗口的高度
      function getWindowHeight() {
        var windowHeight = 0;
        if (document.compatMode == "CSS1Compat") {
          windowHeight = document.documentElement.clientHeight;
        } else {
          windowHeight = document.body.clientHeight;
        }
        return windowHeight;
      }
      const getData = jieLiu(that.getList);

      window.onscroll = function() {
        if (getScrollTop() + getWindowHeight() >= getScrollHeight() - 1.5) {
          if (that.page <= Math.floor(that.total / that.pageSize)) {
            getData();
          }
        }
      };

      //重置滚动条的位置到上次滚动的位置
      function setOnscroll() {
        window.scrollTo(0, height);
      }
    },

    async getList() {
      if (this.loading) return;
      this.loading = true;
      const res = await this.$axios.post(webUrl + "articleLists", {
        page: this.page,
        pageSize: this.pageSize
      });
      this.loading = false;
      this.items = [...this.items, ...res.data.data];
      this.total = res.data.total;
      this.page = this.page += 1;
    }
  }
};
</script>

<style lang="scss" scoped>
.main {
  background: #f8f8fd;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.4), 0 0 30px rgba(10, 10, 0, 0.1) inset;
  margin: 0 16px;
  z-index: 55;
  padding: 10px;
  .bottom-text {
    text-align: center;
    margin-top: 6px;
    color: rgb(144, 147, 153);
  }
}
.aside {
  background: #f8f8fd;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.4), 0 0 30px rgba(10, 10, 0, 0.1) inset;
  .card {
    border-top: 1px solid #eee;
    .title {
      padding: 10px;
      font-weight: 600;
      color: #808080;
      margin-bottom: 10px;
    }
    .pic {
      width: 100%;
      padding-top: -40px;
    }
    .row {
      padding: 0 10px;
      & > p {
        color: #bfbfbf;
      }
      .icons {
        padding: 10px 0;
        .iconfont {
          transition: all 0.3s;
          margin: 5px;
          color: #000;
          font-size: 20px;
          background-color: rgba(200, 200, 200, 0.3);
          padding: 8px;
          border-radius: 50%;
          &:hover {
            color: #fff;
            background-color: rgba(0, 133, 166, 0.8);
            border-radius: 5px;
          }
        }
      }

      & > .link {
        color: #bfbfbf;
        display: inline-block;
        padding: 5px;
        transition: all 0.3s;
        &:hover {
          color: #0085a1;
        }
      }
    }
  }
}

@media (min-width: 768px) {
  //pc
  .main {
    margin: 0 20px;
    padding: 20px;
  }
}
</style>
