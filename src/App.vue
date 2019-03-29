<template>
  <div>
    <v-header :seller="seller"></v-header>
    <!--<v-tab></v-tab>-->
    <ul class="tab border-1px">
      <li class="tab-item"><router-link to='/goods'>商品</router-link></li>
      <li class="tab-item"><router-link to='/ratings'>评价</router-link></li>
      <li class="tab-item"><router-link to='/seller'>商家</router-link></li>
    </ul>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script type="text/ecmascript-6">
  import header from './components/header/header';
  // import tab from './components/tabs/tab';
  // 状态语义化
  const ERR_OK = 0;

  export default {
    data(){
      return {
        seller: {}
      }
    },
    created(){
      // GET /someUrl
      this.$http.get('/api/seller').then(response => {
        // get body data
        // this.someData = response.body;
        response = response.body;
        if (response.errno === ERR_OK) {
          this.seller = response.data;
        }
      });
    },
    // 注册
    components: {
        'v-header': header
        // 'v-tab': tab
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../static/css/reset.css";
  @import "./common/stylus/mixin.styl";
  .tab
    display: flex
    width: 100%
    height: 40px
    line-height: 40px
    overflow: hidden
    // border-bottom: 1px solid rgba(7,17,27,0.1)
    border-1px(rgba(7,17,27,0.1))
    .tab-item
      text-align: center
      flex: 1
      font-size: 14px
      color: rgb(77,85,93)
      & > a
          display: block
          &.active
            color: rgb(240, 20, 20)
</style>
