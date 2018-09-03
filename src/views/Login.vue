<template>
  <div class="login">
    <div class="container">
      <img src="../assets/2.jpg" class="avatar" alt="">
      <el-form :model="LoginForm" :rules="rules" ref="LoginForm" class="demo-ruleForm">
        <el-form-item prop="username">
          <el-input v-model="LoginForm.username" prefix-icon="myicon myicon-user" placeholder="用户名"></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input v-model="LoginForm.password" prefix-icon="myicon myicon-key" placeholder="密码"></el-input>
        </el-form-item>

        <el-form-item>
          <el-button type="primary" class="login-btn" @click="submitForm('LoginForm')">登录</el-button>
        </el-form-item>
      </el-form>
    </div>

  </div>
</template>

<script>
import { login } from '@/api/index.js'

export default {
  data () {
    return {
      LoginForm: {
        username: '',
        password: ''
      },
      rules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 2, max: 8, message: '长度在 2 到 8 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 10, message: '长度在 6 到 10 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    submitForm (formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          // alert('验证成功')
          login(this.LoginForm).then((res) => {
            console.log(res)
            if (res.meta.status === 200) {
              // 登录成功消息提示
              this.$message({
                message: res.meta.msg,
                type: 'success'
              })
              // 路由跳转
              this.$router.push({name: 'Home'})
            } else {
              // 登录成功消息提示
              this.$message({
                message: res.meta.msg,
                type: 'error'
              })
            }
          })
        } else {
          return false
        }
      })
    },
    resetForm (formName) {
      this.$refs[formName].resetFields()
    }
  }
}
</script>

<style lang = 'scss' scoped>
.login {
  position: fixed;
  width: 100%;
  height: 100%;
  background-color: #2f4050;
  .container {
    position: absolute;
    left: 0;
    right: 0;
    width: 400px;
    padding: 0px 40px 15px 40px;
    margin: 200px auto;
    background: white;
    .avatar {
      position: relative;
      left: 50%;
      width: 120px;
      height: 120px;
      margin-left: -60px;
      margin-top: -60px;
      box-sizing: border-box;
      border-radius: 50%;
      border: 10px solid #fff;
      box-shadow: 0 1px 5px #ccc;
      overflow: hidden;
    }
    .login-btn {
      width: 100%;
    }
  }
}
</style>
