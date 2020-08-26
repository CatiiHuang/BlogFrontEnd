<template>
  <div class="body">
  <div class="stars">
    <div class="star" ref="star"  v-for="(item,index) in starsCount" :key="index"></div>
  </div>
    <h1 class="tit">Welcome to Mr Div!</h1>
    <div id="sign_wrap">
      <div v-if="status==1" class="main signIn">
        <input v-model="name" placeholder="请输入登录账号">
        <input v-model="password" @keyup.enter="signIn" placeholder="请输入密码" type="password">
        <el-button @click="signIn" class="sign" >Sign In</el-button>
        <p class="toSignUp"><span @click="toSignUp"  style="float: right"> Sign Up !</span></p>
      </div>
      <div v-else-if="status==2" class="main signUp">
        <input v-model="name" placeholder="请输入登录账号">
        <input v-model="nickName" placeholder="请输入昵称">
        <input v-model="password" placeholder="请输入密码" type="password">
        <el-button @click="signUp" class="sign" >Sign Up</el-button>
        <p class="toSignUp"><span @click="back" > Back to Sign In !</span></p>
      </div>
      <div v-else-if="status==3" class="loading">
        <div v-if="!txtSignIn" class="outside"></div>
        <p v-else class="txt">Sign In</p>
      </div>
    </div>
  </div>
</template>

<script>
import { webUrl } from "../../static/js/public.js";

export default {
  created() {},
  data() {
    return {
      starsCount: 800,
      distance: 800,
      status: 1, //1登录,2注册，3loading
      name: "",
      password: "",
      nickName: "",
      txtSignIn: false
    };
  },
  mounted() {
    let _this = this;
    // 原生js
    let _starList = document.getElementsByClassName("star")
    let starArr = Array.prototype.slice.call(_starList)
    // vue
    let starList = this.$refs.star
    // 遍历添加样式
    starArr.forEach(item => {
      var s = 0.2 + (Math.random() * 1);
      var thisDistance = _this.distance + (Math.random() * 300);
      item.style.transformOrigin = `0 0 ${thisDistance}px`
      item.style.transform = `translate3d(0,0,-${thisDistance}px) rotateY(${(Math.random() * 360)}deg) rotateX(${(Math.random() * -50)}deg) scale(${s},${s})`
    })
  },
  methods: {
    toSignUp: function() {
      this.status = 2;
      this.reset();
    },
    back: function() {
      this.status = 1;
      this.reset();
    },
    reset: function() {
      this.name = "";
      this.password = "";
      this.nickName = "";
    },
    signUp: function() {
      //注册
      let that = this;
      console.log(that.name.length);
      if (that.name.length < 5) {
        that.$message({
          type: "warning",
          message: "登录账号最短需要5位哦！"
        });
        return;
      }
      if (that.name.length > 20) {
        that.$message({
          type: "warning",
          message: "登录账号太长!"
        });
        return;
      }
      if (that.name.length > 20) {
        that.$message({
          type: "warning",
          message: "登录账号太长!"
        });
        return;
      }
      if (that.nickName.length > 12) {
        that.$message({
          type: "warning",
          message: "昵称太长!"
        });
        return;
      }
      if (
        that.name.length == 0 ||
        that.nickName.length == 0 ||
        that.password.length == 0
      ) {
        that.$message({
          type: "warning",
          message: "有未填写项!"
        });
        return;
      }
      that.$axios
        .post(webUrl + "admin/signUp", {
          name: that.name,
          password: that.password,
          nickName: that.nickName
        })
        .then(response => {
          that.$message({
            type: "success",
            message: response.data.msg
          });
          if (response.data.status == 1) {
            that.back();
          }
        })
        .catch(reject => {
          console.log(reject);
        });
    },
    signIn: function() {
      //登录
      let that = this;
      this.$axios
        .post(webUrl + "admin/signIn", {
          name: this.name,
          password: this.password
        })
        .then(response => {
          if (response.data.status == 1) {
            let type = response.data.type;
            this.status = 3;
            localStorage.setItem("token", response.data.token);
            localStorage.setItem("user_name", response.data.user_name);
            localStorage.setItem("nickName", response.data.nickName);
            localStorage.setItem("avatar", response.data.avatar);
            if (type == 1) {
              this.$store.commit("changeIsSignIn", 1); //admin
            } else if (type == 2) {
              this.$store.commit("changeIsSignIn", 2); //游客
              this.$store.commit("changeIndex", "7");
            }
            setTimeout(() => {
              this.txtSignIn = true;
              setTimeout(() => {
                if (type == 1) {
                  this.$router.push({ name: "artList" }); //admin
                } else if (type == 2) {
                  this.$router.push({ name: "visiter" }); //游客
                }
              }, 1000);
            }, 2000);
          } else {
            that.$message({
              type: "error",
              message: response.data.msg
            });
          }
        })
        .catch(reject => {
          console.log(reject);
        });
    }
  },
  beforeCreate: function() {
    document.getElementsByTagName("body")[0].className = "admin";
  },
  beforeDestroy: function() {
    document.body.removeAttribute("class", "admin");
  }
};
</script>

<style scoped lang='scss'>
  .body {
    min-height:80vh;
    padding-top: 24vh;
    background: radial-gradient(200% 100% at bottom center, #f7f7b6, #e96f92, #75517d, #1b2947);
    background: radial-gradient(220% 105% at top center, #1b2947 10%, #75517d 40%, #e96f92 65%, #f7f7b6);
    background-attachment: fixed;
    overflow: hidden;
  }
  .tit{
    text-align: center;
    margin: 60px 0;
    color: #ecf5ff;
    font-size: 40px;
    font-weight: 500 ;
  }
  @keyframes rotate {
    0% {
      transform: perspective(400px) rotateZ(20deg) rotateX(-40deg) rotateY(0);
    }
    100% {
      transform: perspective(400px) rotateZ(20deg) rotateX(-40deg) rotateY(-360deg);
    }
  }

  .stars {
    transform: perspective(500px);
    transform-style: preserve-3d;
    position: absolute;
    bottom: 0;
    perspective-origin: 50% 100%;
    left: 50%;
    animation: rotate 90s infinite linear;
  }

  .star {
    width: 2px;
    height: 2px;
    background: #F7F7B6;
    position: absolute;
    top: 0;
    left: 0;
    backface-visibility: hidden;
  }
#sign_wrap {


  width: 300px;
  margin: 0 auto 0;
  input {
    background: hsla(0, 0%, 100%, 0.08);
    border-radius: 4px;
    border: 1px solid #dcdfe6;
    box-sizing: border-box;
    color: hsla(0, 0%, 100%, 0.58);
    display: inline-block;
    font-size: inherit;
    height: 40px;
    line-height: 1;
    outline: 0;
    padding: 0 15px;
    transition: all 0.25s ease;
    width: 100%;
    margin-bottom: 20px;
    &:focus {
      border-color: #409eff;
      background: hsla(0, 0%, 100%, 0.28);
    }
    &::-webkit-input-placeholder {
      color: hsla(0, 0%, 100%, 0.58);
    }
    &::-moz-placeholder {
      color: hsla(0, 0%, 100%, 0.58);
    }
    &:-moz-placeholder {
      color: hsla(0, 0%, 100%, 0.58);
    }
    &:-ms-input-placeholder {
      color: hsla(0, 0%, 100%, 0.58);
    }
  }
  .sign {
    transition: all 0.25s ease;
    background: hsla(0, 0%, 100%, 0.08);
    border: 1px solid hsla(0, 0%, 100%, 0.65);
    border-radius: 3px;
    box-shadow: 0 0 8px hsla(0, 0%, 100%, 0.3);
    color: #fff;
    display: inline-block;
    font-size: 2rem;
    padding: 0.78rem 1.3rem;
    text-decoration: none;
    text-shadow: none;
    width: 300px;
    &:hover {
      background: hsla(0, 0%, 100%, 0.28);
    }
  }
  .toSignUp {
    color: #ddd;
    text-align: center;
    margin: 20px 0;
    font-size: 18px;
    > span {
      color: #ecf5ff;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.3s;
      &:hover {
        color: #5da9ff;
      }
    }
  }
  .loading {
    width: 100px;
    height: 100px;
    position: relative;
    margin: 50px auto;
    .outside {
      border: 4px solid rgba(255, 255, 255, 0.7);
      border-radius: 50%;
      animation: loadOut 1s infinite;
      position: absolute;
      left: 50%;
      top: 50%;
    }
    .txt {
      color: rgba(255, 255, 255, 0);
      animation: txtLarge 1.5s;
      text-align: center;
      line-height: 100px;
    }
  }
  @keyframes loadOut {
    from {
      width: 0;
      height: 0;
      margin-left: 0;
      margin-top: 0;
    }
    to {
      width: 92px;
      height: 92px;
      margin-left: -50px;
      margin-top: -50px;
      opacity: 0;
    }
  }
  @keyframes txtLarge {
    from {
      transform: scale(1);
      color: rgba(255, 255, 255, 0);
    }
    to {
      transform: scale(3);
      color: rgba(255, 255, 255, 0.6);
    }
  }
}
  @media screen and (max-width: 1000px) {
    .body {
      padding-top: 6vh;
      min-height:94vh;
    }
  }
</style>
