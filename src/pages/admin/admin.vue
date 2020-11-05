<template>
  <div>
    <el-menu
      :default-active="activeIndex2"
      class="el-menu-demo"
      router
      mode="horizontal"
      @select="handleSelect"
      background-color="#545c64"
      text-color="#fff"
      default-active="/admin/artList"
      active-text-color="#ffd04b"
    >
      <span class="adminTilte">Catii ,</span>
      <el-menu-item index="/admin/artList">文章列表</el-menu-item>
      <el-menu-item index="/admin/demoList">Demo列表</el-menu-item>
      <!--    <el-menu-item index="/admin/musicList">音乐列表</el-menu-item>-->
      <el-menu-item style="float: right" index="/">返回首页</el-menu-item>
    </el-menu>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: "admin",
  data() {
    return {
      activeIndex: "1",
      activeIndex2: "1"
    };
  },
  methods: {
    handleSelect(key, keyPath) {
      // console.log(key, keyPath);
    }
  }
};
</script>

<style scoped>
.adminTilte {
  display: inline-block;
  float: left;
  padding: 0 26px;
  font-size: 20px;
  margin-right: 30px;
  line-height: 60px;
}
</style>
