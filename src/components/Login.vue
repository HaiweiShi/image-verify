<template>
  <div class="hello">
    <div class="input-item">
      <input v-model="user" type="text" placeholder="请输入账号">
    </div>
    <div class="input-item">
      <input v-model="pwd" type="password" placeholder="请输入密码">
    </div>
    <image-verify v-model="valid">
      <div>valid：<span>{{ valid ? '验证成功' : '点击验证' }}</span></div>
    </image-verify>
    <button @click="login">登录</button>
  </div>
</template>

<script>
import ImageVerify from './ImageVerify'

export default {
  name: 'Login',
  components: {
    'image-verify': ImageVerify
  },
  inject: ['setName'],
  data () {
    return {
      user: '',
      pwd: '',
      valid: false
    }
  },
  methods: {
    login () {
      if (!this.user) { alert('请输入账号'); return }
      if (!this.pwd) { alert('请输入密码'); return }
      if (!this.valid) { alert('请点击验证'); return }

      let user = window.sessionStorage.getItem(this.user)
      if (!user) {
        alert('请先注册')
      }
      try {
        user = JSON.parse(user)
        if (user.pwd === this.pwd) {
          this.setName(user.name)
          alert('登录成功')
          this.$router.replace('/helloWorld')
        } else {
          alert('密码错误')
        }
      } catch (error) {
        alert('登录失败')
      }
    }
  }
}
</script>
