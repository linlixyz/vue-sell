<template>
  <div class="shopcart">
    <div class="content">
      <div class="content-left">
        <div class="logo-wrapper">
          <div class="logo" :class="{'highlight': totalCount>0}">
            <i class="icon-shopping_cart"></i>
          </div>
          <div class="num" v-show="totalCount>0">{{totalCount}}</div>
        </div>
        <div class="price" :class="{'highlight': totalPrice>0}">￥{{totalPrice}}</div>
        <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
      </div>
      <div class="content-right">
        <div class="pay" :class="payClass">{{payDesc}}</div>
      </div>
    </div>
    <div class="ball-container">
      <div v-for="(ball,index) in balls" v-show="ball.show" :key="index" class="ball">
        <transition name="drop">
          <div class="inner"></div>
        </transition>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
      // 数据驱动 所有的计算方法都直接或间接依赖 selectFoods,将复杂的状态判断，都集中在selectFoods的返回值上
      selectFoods: {
        type: Array,
        default(){
          return [
            {
              price: 10,
              count: 2
            }
          ]
        }
      },
      deliveryPrice: {
        type: Number,
        default: 0
      },
      minPrice: {
        type: Number,
        default: 0
      }
    },
    data(){
      return {
        balls: [
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          }
        ]
      }
    },
    computed: {
      totalPrice(){
        let total = 0;
        this.selectFoods.forEach((food) => {
          total += food.price * food.count
        })
        return total;
      },
      totalCount() {
        let count = 0;
        this.selectFoods.forEach((food) => {
          count += food.count
        })
        return count;
      },
      payDesc() {
        if (this.totalPrice === 0){
          return `￥${this.minPrice}元起送`;
        } else if (this.totalPrice < this.minPrice){
          let payprice = this.minPrice - this.totalPrice;
          return `还差￥${payprice}元起送`;
        } else {
          return '去结算';
        }
      },
      payClass() {
        if (this.totalPrice < this.minPrice){
          return "not-enough"
        } else {
          return "enough"
        }
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .shopcart
    position: fixed
    left: 0
    bottom: 0
    z-index: 50
    width:100%
    height: 48px
    background: #141d27
    .content
      display: flex
      color: rgba(255,255,255,0.4)
      .content-left
        flex: 1
        .logo-wrapper
          display: inline-block
          position: relative
          top: -10px
          margin: 0px 12px
          padding: 6px
          background: #141d27
          width: 56px
          height: 56px
          box-sizing: border-box
          vertical-align: top
          border-radius: 50%
          .logo
            width: 100%
            height: 100%
            background: #2b343c
            border-radius: 50%
            text-align: center
            &.highlight
              background: rgb(0,160,220)
              .icon-shopping_cart
                color: #fff
            .icon-shopping_cart
              line-height: 44px
              font-size: 24px
              color: #80858a
          .num
            position: absolute
            top: 0
            right: 0
            width: 24px
            height: 16px
            line-height: 16px
            text-align: center
            border-radius: 16px
            font-size: 10px
            font-weight: 700
            color: #fff
            background: rgb(240,40,40)
            box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4)
        .price
          display: inline-block
          margin-top: 12px
          padding-right: 12px
          font-weight: 700
          line-height: 24px
          font-size: 16px
          border-right: 1px solid rgba(255,255,255,0.1)
          box-sizing: border-box
          &.highlight
            color: #fff
        .desc
          display: inline-block
          padding-left:12px
          line-height: 24px
          font-size: 12px
      .content-right
        flex: 0 0 105px
        width: 105px
        .pay
          height: 48px
          line-height: 48px
          font-size: 10px
          font-weight: 700
          text-align: center
          &.not-enough
            background: #2b333b
          &.enough
            background: #00b43c
            color: #fff
    .ball-container
      .ball
        position: fixed
        left: 32px
        bottom: 22px
        z-index: 200
        &.drop-transition
          transition: all 0.4s
          .inner
            width: 16px
            height: 16px
            border-radius: 50%
            background: rgb(0,160,220)
</style>
