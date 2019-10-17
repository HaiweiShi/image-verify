<template>
  <div style="text-align:left;width:600px;display: inline-block;">
    <h1 style="text-align:center">图片验证</h1>
    <h3>使用方法</h3>
    <ul>
      <li>
        <div style="text-align:left;display: inline-block;">
          import ImageVerify from './ImageVerify'<br /><br />
          &lt;image-verify v-model="valid"&gt;<br />
          &nbsp;&nbsp;&lt;div&gt;{{ valid ? '验证成功' : '点击验证' }}&lt;/div&gt;<br />
          &lt;/image-verify&gt;<br /><br /><br />
        </div>
      </li>
    </ul>
    <h3>Attribute</h3>
    <table border>
      <thead>
        <tr>
          <th>属性名</th>
          <th>说明</th>
          <th>类型</th>
          <th>默认值</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>value</td><td>value</td><td>Boolean</td><td>false</td>
        </tr>
        <tr>
          <td>imageList</td><td>可选图片列表</td><td>Array</td><td>[...]</td>
        </tr>
        <tr>
          <td>zIndex</td><td>弹框的z-index</td><td>Number</td><td>2019</td>
        </tr>
        <tr>
          <td>float</td><td>是否弹框</td><td>Boolean</td><td>true</td>
        </tr>
        <tr>
          <td>timeout</td><td>用时限制，为0时不校验</td><td>Number</td><td>0</td>
        </tr>
        <tr>
          <td>width</td><td>宽度，最小300</td><td>Number</td><td>368</td>
        </tr>
        <tr>
          <td>height</td><td>高度，最小162</td><td>Number</td><td>200</td>
        </tr>
        <tr>
          <td>areaSize</td><td>拖动区域大小</td><td>Number</td><td>50</td>
        </tr>
      </tbody>
    </table>
    <h3>Methods</h3>
    <table border>
      <thead>
        <tr>
          <th>方法名</th>
          <th>说明</th>
          <th>参数</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>showModel</td><td>弹出验证框，float为true时有效</td><td>无</td>
        </tr>
        <tr>
          <td>closeModel</td><td>关闭验证框，float为true时有效</td><td>无</td>
        </tr>
        <tr>
          <td>resetValidArea</td><td>重置验证状态</td><td>无</td>
        </tr>
      </tbody>
    </table>
    <h3>Events</h3>
    <table border>
      <thead>
        <tr>
          <th>事件名</th>
          <th>说明</th>
          <th>回调参数</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>input</td><td></td><td>function(valid)</td>
        </tr>
        <tr>
          <td>change</td><td>拖动方块后触发，返回验证结果和用时</td><td>function(valid,time)</td>
        </tr>
      </tbody>
    </table>
    <h3>实例</h3>
    <router-link to="/login">登录页</router-link>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
    }
  }
}
</script>

<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
table{
  width: 600px;
  border:1px solid #ddd;
  border-collapse: collapse;
}
table tr {
  height: 30px;
}
table tr td,table tr th {
  padding-left: 10px;
}
</style>
