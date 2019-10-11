<template>
  <div>
    <h3>我是子组件一</h3>
    <span>{{parentMessage}}</span>
    <button @click="sendMsg">向父组件传参</button>  <!-- 这里用简单的绑定方法触发传参-->
  </div>
</template>
<script>
  export default{
      props: ['parentMessage'],
      methods: {
        sendMs() { //传参方法
          this.$emit('headCallBack', '子组件的参数内容'); //第一个参数是父组件中v-on绑定的自定义回调方法，第二个参数为传递的参数
        }
    }

  };
</script>
<style scoped>
</style>