<template>
    <div class="cont">
      <h1>音乐</h1>
      <el-form :model="Form" status-icon  ref="ruleForm" label-width="100px" class="demo-ruleForm">
        <el-form-item label="姓名" prop="pass">
          <el-input type="text" v-model="Form.name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="性别" prop="checkPass">
          <el-input type="sex" v-model="Form.sex" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="年龄" prop="age">
          <el-input v-model.number="Form.age"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitForm(Form)">提交</el-button>
          <el-button @click="resetForm(Form)">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
</template>

<script>
    export default {
        name: "musicList",
      data(){
        return{
          Form:{
            name:"",
            age:"",
            sex:""
          }
        }
      },
      methods:{
        submitForm(form) {
          console.log(form);

        },
        resetForm() {
         this.Form.name = "";
         this.Form.age = "";
         this.Form.sex = "";
        }
      }
    }
</script>

<style scoped>
.cont{
  width: 1200px;
  margin: 100px auto;
}
</style>
