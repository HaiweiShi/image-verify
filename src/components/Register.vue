<template>
  <div class="hello">
    <div class="input-item">
      <input v-model="user" type="text" placeholder="请输入账号">
    </div>
    <div class="input-item">
      <input v-model="pwd" type="password" placeholder="请输入密码">
    </div>
    <image-verify v-model="valid" :float="false" >
      <div>valid：<span>{{ valid ? '验证成功' : '未验证' }}</span></div>
    </image-verify>
    <button @click="register">注册</button>
  </div>
</template>

<script>
import ImageVerify from './ImageVerify'

export default {
  name: 'Register',
  components: {
    'image-verify': ImageVerify
  },
  data () {
    return {
      user: '',
      pwd: '',
      valid: false
    }
  },
  methods: {
    register () {
      if (!this.user) { alert('请输入账号'); return }
      if (!this.pwd) { alert('请输入密码'); return }
      if (!this.valid) { alert('请点击验证'); return }
      let user = {
        name: this.user,
        pwd: this.pwd
      }
      user = JSON.stringify(user)
      try {
        window.sessionStorage.setItem(this.user, user)
        alert('注册成功')
        this.$router.replace('/login')
      } catch (error) {
        alert('注册失败')
      }
    }
  }
}
</script>
