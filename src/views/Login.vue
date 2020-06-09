<template>
  <div class="container-login">
    <el-card class="my-card">
      <img src=".././assets/logo_index.png" />
      <el-form :model="loginForm" :rules="loginRules" ref="loginForm">
        <el-form-item>
          <el-input
            type="text"
            v-model="loginForm.mobile"
            placeholder="请输入手机号"
            prop="mobile"
          ></el-input>
        </el-form-item>
        <el-form-item class="formitem">
          <el-input
            type="text"
            v-model="loginForm.code"
            placeholder="验证码"
            style="width: 240px;margin-right:8px"
            prop="code"
          ></el-input>
          <el-button>添加验证码</el-button>
        </el-form-item>
        <el-form-item>
          <!-- :value="true" 选中复选框不能进行修改  必须同意 -->
          <el-checkbox :checked="true"
            >我已阅读并同意用户协议和隐私条款</el-checkbox
          >
        </el-form-item>
        <el-form-item label>
          <el-button type="primary" @click="login()" style="width:100%"
            >登录</el-button
          >
        </el-form-item>
      </el-form>
    </el-card>
  </div>
</template>

<script>
export default {
  //   name: 'login-page',
  data () {
    const checkMobile = (rule, value, callback) => {
      // 进行校验 （1开头，第二位 3-9 数字，其余剩下9位数字）
      if (!/^1[3-9]\d{9}$/.test(value)) {
        // 校验失败
        callback(new Error('手机号格式错误'))
      } else {
        // 校验成功
        callback()
      }
    }
    return {
      // 表单对应的数据对象
      loginForm: {
        mobile: '13911111111',
        code: '246810'
      },
      // 表单对应的校验规则
      loginRules: {
        mobile: [
          // required 是否必填
          // message 错误提示
          // trigger 触发校验时机
          { required: true, message: '请输入手机号', trigger: 'blur' },
          // 手机号格式校验没有提供  需要实现自定义校验
          { validator: checkMobile, trigger: 'blur' }
        ],
        code: [
          { required: true, message: '请输入验证码', trigger: 'blur' },
          // len 输入内容必须是 6位  min max 指定字符串的范围
          { len: 6, message: '验证码6个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    login () {
      // 1. 对整体表单进行校验
      this.$refs.loginForm.validate((valid) => {
        if (valid) {
          // 2. 校验成功后进行登录
          this.$http.post('/authorizations', this.loginForm).then(() => {
            // 成功
            this.$router.push('/home')
          }).catch(() => {
            // 提示
            this.$message.error('手机号或验证码错误')
          })
        }
      })
    }
  }
}
</script>

<style lang="less" scoped>
.container-login {
  width: 100%;
  height: 100%;
  background: url(.././assets/login_bg.jpg);
  position: absolute;
  top: 0;
  left: 0;
  .my-card {
    transform: translate(-50%, -60%);
    position: absolute;
    width: 400px;
    height: 350px;
    top: 50%;
    left: 50%;
    img {
      width: 200px;
      margin: 0 auto 20px;
    }
  }
}
</style>
