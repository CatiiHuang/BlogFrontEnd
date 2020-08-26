<template>
  <div id="content">
<!--    <a @click="toHome" href="javascript:;" class="toHome">返回首页</a>-->
    <h1 class="title">Demo列表</h1>




      <el-button  @click="handleAdd2()" class="btn-add">新增+</el-button>
      <el-table
        :data="demoList"
        style="width: 100%"
        header-align='right'
        border
        stripe>
        <el-table-column
          label="标题"
          width="200">
          <template slot-scope="scope">
            <span>{{ scope.row.title }}</span>
          </template>
        </el-table-column>
        <el-table-column
          label="日期"
          width="200">
          <template slot-scope="scope">
            <i class="el-icon-time"></i>
            <span>{{ scope.row.date }}</span>
          </template>
        </el-table-column>
        <el-table-column
          label="file"
          width="200">
          <template slot-scope="scope">
            <span>{{ scope.row.file }}</span>
          </template>
        </el-table-column>
        <el-table-column
          label="图片"
          width="200">
          <template slot-scope="scope">
            <span>{{ scope.row.pic }}</span>
          </template>
        </el-table-column>
        <el-table-column
          label="摘要"
          width="200">
          <template slot-scope="scope">
            <span>{{ scope.row.gist.slice(0,30) }}</span>
          </template>
        </el-table-column>
        <el-table-column label="操作">
          <template slot-scope="scope">
            <el-button
              size="mini"
              type="primary"
              @click="handleLook2(scope.$index, scope.row)">查看</el-button>
            <el-button
              size="mini"
              type="success"
              @click="handleEdit2(scope.$index, scope.row)">编辑</el-button>
            <el-button
              size="mini"
              type="danger"
              @click="handleDelete2(scope.$index, scope.row)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>

  </div>
</template>

<script>
  import { checkAdmin } from "../../../static/js/public.js";
  import { webUrl } from "../../../static/js/public.js";

  export default {
    mixins: [checkAdmin],
    data() {
      return {
        articleList: [],
        demoList: [],
        type: "article"
      };
    },
    beforeCreate: function() {},
    mounted: function() {
      // 获取文章列表
      this.$axios.post(webUrl + "articleList").then(res => {
        if (res) {
          this.articleList = res.data.reverse();
        }
      });
      // 获取demo列表
      this.$axios.post(webUrl + "demoList").then(res => {
        if (res) {
          this.demoList = res.data.reverse();
        }
      });
    },
    methods: {
      toHome: function() {
        this.$router.replace({ name: "home" });
      },
      toggle() {
        //切换
        this.type = this.type == "article" ? "demo" : "article";
      },
      handleAdd() {
        //新增
        this.$router.push("/admin/edit");
      },
      handleAdd2() {
        //新增-demo
        this.$router.push("/admin/editt");
      },
      handleLook(index, row) {
        //查看
        let id = row._id;
        window.open("#/detail/" + id);
      },
      handleLook2(index, row) {
        //查看-demo
        window.open("#/demo/");
      },
      handleEdit(index, row) {
        //编辑
        let id = row._id;
        this.$router.push("/admin/edit/" + id);
      },
      handleEdit2(index, row) {
        //编辑-demo
        let id = row._id;
        this.$router.push("/admin/editt/" + id);
      },
      handleDelete(index, row) {
        //删除
        let self = this;
        let _id = row._id;
        this.$confirm("此操作将永久删除该文章, 是否继续?", "提示", {
          confirmButtonText: "确定",
          cancelButtonText: "取消",
          type: "warning"
        })
          .then(() => {
            self.$axios
              .post(webUrl + "admin/deleteArticle", { _id: _id })
              .then(res => {
                if (res.data.status == 1) {
                  self.$message({
                    type: "success",
                    message: "删除成功!"
                  });
                  self.fetchData();
                }
              });
          })
          .catch(() => {
            this.$message({
              type: "info",
              message: "已取消删除"
            });
          });
      },
      handleDelete2(index, row) {
        //删除--demo
        let self = this;
        let _id = row._id;
        this.$confirm("此操作将永久删除该文章, 是否继续?", "提示", {
          confirmButtonText: "确定",
          cancelButtonText: "取消",
          type: "warning"
        })
          .then(() => {
            self.$axios
              .post(webUrl + "admin/deleteDemo", { _id: _id })
              .then(res => {
                if (res.data.status == 1) {
                  self.$message({
                    type: "success",
                    message: "删除成功!"
                  });
                  self.fetchData2();
                }
              });
          })
          .catch(() => {
            this.$message({
              type: "info",
              message: "已取消删除"
            });
          });
      },
      fetchData: function() {
        // 更新数据
        this.$axios.post(webUrl + "articleList").then(res => {
          if (res) {
            this.articleList = res.data.reverse();
          }
        });
      },
      fetchData2: function() {
        // 更新数据---demo
        this.$axios.post(webUrl + "demoList").then(res => {
          if (res) {
            this.demoList = res.data.reverse();
          }
        });
      }
    }
  };
</script>


<style>
  .el-table .cell {
    text-align: center;
  }
</style>
<style lang="scss" scoped>
  #content {
    width: 1300px;
    margin: 0 auto;
    > .title {
      margin: 30px 0;
      text-align: center;
    }
    .tab-box {
      text-align: center;
      margin-bottom: 20px;
    }
    .btn-add {
      float: right;
      margin-bottom: 20px;
    }
  }
</style>
