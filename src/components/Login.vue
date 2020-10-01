<template>
  <div class="login_container">
    <div class="login_box">
      <!-- 头像区 -->
      <div class="avatar_box">
        <img src="../assets/logo.png" alt="">
      </div>
      <!-- 登录表单区 -->
      <el-form ref="loginFormRef" :model='loginForm' :rules="loginFormRules" class="login_form">
        <!-- 用戶名 -->
        <el-form-item prop="username">
          <el-input v-model="loginForm.username" prefix-icon="iconfont icon-user"></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input v-model="loginForm.password" prefix-icon="iconfont icon-3702mima" type="password"></el-input>
        </el-form-item>
        <!-- 按钮 -->
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item >
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      // 这是登录表单的数据绑定对象
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      // 这是表单的验证规则对象
      loginFormRules: {
        // 验证用户名是否合法
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ],
        // 验证密码是否合法
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 16, message: '长度在 6 到 16 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    // 点击重置按钮，重置登录表单
    resetLoginForm () {
      // console.log(this)
      this.$refs.loginFormRef.resetFields()
    },
    // 登录验证
    login () {
      this.$refs.loginFormRef.validate(async (valid) => {
        if (!valid) {}
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return this.$message.error('登录失败')
        this.$message.success('登录成功')
        // 将token保存在sessionStorage中,作用是当用户关闭页面的时候，再次打开可以免登录
        window.sessionStorage.setItem('token', res.data.token)
        // 通过编程式导航转到后台主页
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

  // 实现垂直居中
  position: absolute;
  left: 50%;
  top: 50%;
  // 需要减去自身的大小
  transform: translate(-50%,-50%);

  .avatar_box {
    height: 130px;
    width: 130px;
    // 增加圆角
    border: 1px solid #eee;
    // 是边框为圆角
    border-radius: 50%;
    // 使图片和边框产生距离
    padding: 10px;
    // box-shadow: h-shadow(水平) v-shadow(垂直) blur(模糊距离) spread(尺寸) color inset(内部阴影);
    box-shadow: 0 0 10px ;
    // 进行定位
    position: absolute;
    left: 50%;
    transform: translate(-50%,-50%);
    // 去掉外边的蓝色边框
    background-color: #fff;
    img {
      width: 100%;
      height: 100%;
      // 使图片变为圆角
      border-radius: 50%;
      background-color: #eee;
    }
  }
}

.login_form {
  position: absolute;
  bottom: 20px;
  width: 100%;
  // 两边留一定的位置
  padding: 0 20px;
  box-sizing: border-box;
}

.btns {
  display: flex;
  justify-content: flex-end;
}
</style>
