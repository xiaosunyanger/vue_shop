<template>
    <div class="login_container">
    <div class="login_box">
        <div class="avater-box">
            <img src= "./../assets/logo.png" alt="">
        </div>
        <el-form ref="Loginform"  :model= "loginForm" :rules="rules" label-width="0px" class="login_form">
            <el-form-item prop= "username">
                <el-input prefix-icon="iconfont icon-user"  v-model= "loginForm.username"></el-input>
            </el-form-item>
            <el-form-item prop= "password">
                <el-input prefix-icon="iconfont icon-3702mima"
                v-model= "loginForm.password" type="password"></el-input>
            </el-form-item>
            <el-form-item class="btns">
                <el-button type="primary" @click="login">登录</el-button>
                <el-button type="info" @click="resetLoginForm">重置</el-button>
            </el-form-item>
        </el-form>
    </div>
    </div>
</template>
<script>
export default {
  name: 'Login',
  data () {
    return {
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      rules: {
        username: [
          { required: true, message: '用户名', trigger: 'blur' },
          { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    resetLoginForm () {
      this.$refs.Loginform.resetFields()
    },
    login () {
      this.$refs.Loginform.validate(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return this.$message.error('登陆失败')
        this.$message.success('登陆成功')
        window.sessionStorage.setItem('token', res.data.token)
        this.$router.push('/home')
      })
    }
  }
}
</script>
<style lang="less" scoped>
.login_container{
    background-color: #2b4b6b;
    height: 100%
}

.login_box{
    width: 450px;
    height: 300px;
    background-color: #fff;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
    border-radius: 3px;

    .avater-box{
        width: 130px;
        height: 130px;
        border: 1px solid #eee;
        padding: 10px;
        box-shadow: 0 0 10px #ddd;
        border-radius: 50%;
        position: absolute;
        left: 50%;
        transform: translate(-50%,-50%);
        background-color: #fff;

        img{
            width: 100%;
            height: 100%;
            border: 50%;
            background-color: #eee;
            border-radius: 50%;
        }
    }

    .login_form{
        width: 100%;
        padding: 0 20px;
        position: absolute;
        bottom: 0;
        box-sizing: border-box;

        .btns{
            display: flex;
            justify-content: flex-end;
        }
    }
}
</style>
