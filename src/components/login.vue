<template>
  <div class="login_container">
    <div class="login_box">
      <!-- 头像 -->
      <div class="avatar_box">
        <img src="../assets/logo.png">
      </div>

      <!-- 表单 -->
      <el-form class="form" :model="loginForm" :rules="loginFormRules" ref="loginFormRef">

        <!-- 用户名 -->
        <el-form-item prop="username">
          <el-input prefix-icon="iconfont icon-tubiao211" v-model="loginForm.username"></el-input>
        </el-form-item>

        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input prefix-icon="iconfont icon-mima" v-model="loginForm.password" type="password"></el-input>
        </el-form-item>

        <!-- 登录和重置按钮的盒子 -->
        <div class="btn_container">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </div>

      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      loginForm: {
        username: '',
        password: ''
      },
      loginFormRules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 3, max: 8, message: '长度在 3 到 8 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    resetLoginForm () {
      this.$refs.loginFormRef.resetFields()
    },
    login () {
      this.$refs.loginFormRef.validate(async validate => {
        if (!validate) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) { // 请求失败
          return this.$message.error('登录失败')
        }
        // console.log(res)
        // 存储token
        window.sessionStorage.setItem('token', res.data.token)

        // 跳转到home页面
        this.$router.push('/home')
      })
    }
  }
}
</script>

<style lang="less" scoped>
  .login_container {
    background-color: #2b4b6b;
    height: 100%;
  }
  .login_box {
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 3px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    .avatar_box {
      width: 130px;
      height: 130px;
      border: 1px solid #eee;
      border-radius: 50%;
      padding: 10px;
      box-shadow: 0 0 10px #ddd;
      position: absolute;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: #fff;
      img {
        width: 100%;
        height: 100%;
        border-radius: 50%;
        background-color: #eee;
      }
    }
  }

  .btn_container {
    display: flex;
    justify-content: flex-end;
  }

  .form {
    width: 100%;
    position: absolute;
    bottom: 10px;
    padding: 30px;
    box-sizing: border-box;
    font-size: 22px;
  }
</style>
