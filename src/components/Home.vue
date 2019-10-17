<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h3>查看详情需先<router-link to="/login">登录</router-link></h3>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data () {
    return {
      msg: '首页'
    }
  }
}
</script>

<style scoped>
</style>
