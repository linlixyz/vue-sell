<template>
  <div>
    <h1>{{msg}}</h1>
  </div>
</template>
// 初始化 导出对象 vue的各种对象，定义，属性
<script>
  export default {
    data () {
      return {
        msg: 'Hello vue!'
      };
    }
  };
</script>

<style scoped>

</style>
