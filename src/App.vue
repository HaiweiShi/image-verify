<template>
  <div id="app">
    <div class="header">
      <router-link to="/">首页</router-link>
      <router-link to="/helloWorld">介绍</router-link>
      <router-link to="/login">登录</router-link>
      <router-link to="/register">注册</router-link>
      <span>{{userName || '游客543'}}，你好</span>
    </div>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App',
  provide () {
    return {
      setName: this.setName,
      getName: this.getName
    }
  },
  data () {
    return {
      userName: ''
    }
  },
  methods: {
    setName (name) {
      this.userName = name
    },
    getName (name) {
      return this.userName
    }
  }
}
</script>
<style>
a {
  color: #42b983;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
<style lang="scss">
.header {
  margin-top: 10px;
  margin-bottom: 40px;
  a {
    margin-right:10px;
  }
}
.input-item{
  color: #42b983;
  input{
    width: 368px;
    height: 26px;
    font-size: 16px;
    margin-bottom: 10px;
  }
}
button {
  width: 368px;
  height: 30px;
}
</style>
