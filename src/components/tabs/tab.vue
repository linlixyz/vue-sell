<template>
  <ul class="tab">
    <li class="tab-item"><router-link to='/goods'>商品</router-link></li>
    <li class="tab-item"><router-link to='/ratings'>评价</router-link></li>
    <li class="tab-item"><router-link to='/seller'>商家</router-link></li>
  </ul>
</template>

<style lang="stylus" rel="stylesheet/stylus">
  .tab
    display : flex
    height: 40px
    line-height: 40px
    overflow: hidden
    .tab-item
      text-align: center
      flex: 1
      font-size: 28px
      color: rgb(77,85,93)

</style>
