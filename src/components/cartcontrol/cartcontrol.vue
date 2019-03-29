<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="cart-decrease"
           v-show="food.count>0" @click="decreaseCart($event)">
        <span class="inner icon-remove_circle_outline"></span>
      </div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add" @click="addCart($event)">
      <span class="inner icon-add_circle"></span>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue';

  export default {
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addCart(event) {
        if (!event._constructed){
          return;
        }
        if (!this.food.count){
          Vue.set(this.food,'count',1)
        } else {
          this.food.count++;
        }
      },
      decreaseCart(event) {
        if (!event._constructed){
          return;
        }
        if (this.food.count){
          this.food.count--;
        }
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .cartcontrol
    font-size: 0
    .cart-decrease
      display: inline-block
      /* 通过padding增加可点击区域 */
      padding: 6px
      opacity: 1
      transform: translateX(0)
      .inner
        display: inline-block
        line-height: 24px
        font-size: 24px
        color: rgb(0, 160, 220)
        transition: all 0.4s linear
        transform: rotate(0)
      &.move-enter-active, &.move-leave-active
        transition: all 0.4s linear
      &.move-enter, &.move-leave-active
        opacity: 0
        transform: translateX(24px)
        .inner
          transform: rotate(180deg)
    .cart-count
      display: inline-block
      vertical-align: top
      padding-top: 6px
      width: 12px
      text-align: center
      line-height: 24px
      font-size: 10px
      color: rgb(147,153,159)
    .cart-add
      display: inline-block
      /* 通过padding增加可点击区域 */
      padding: 6px
      .inner
        line-height: 24px
        font-size: 24px
        color: rgb(0,160,220)
</style>
