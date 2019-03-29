<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="(goodsItem,index) in goods" class="goodsItem" :key="index"
            :class="{'current' : currentIndex === index}" @click="selectMenu(index,$event)">
          <span class="text border-1px">
            <span v-show="goodsItem.type>0" class="icon"
                  :class="classMap[goodsItem.type]"></span>{{goodsItem.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <li v-for="(foodsType,index) in goods" class="foodsType food-list-hook" :key="index">
          <span class="foods-type-name">{{foodsType.name}}</span>
          <ul>
            <li v-for="(foodsItem,idx) in foodsType.foods" class="foodsItem border-1px" :key="idx">
              <div class="icon">
                <img width="57" height="57" :src=foodsItem.icon>
              </div>
              <div class="content">
                <p class="name">{{foodsItem.name}}</p>
                <p class="description">{{foodsItem.description}}</p>
                <p class="extra">
                  <span class="sellCount">月售{{foodsItem.sellCount}}份</span><span class="rating">好评率{{foodsItem.rating}}%</span>
                </p>
                <p class="price-box">
                  <span class="price">{{foodsItem.price}}</span><span class="oldPrice" v-show="foodsItem.oldPrice">{{foodsItem.oldPrice}}</span>
                </p>
              </div>
              <div class="cart-wrapper">
                <cartcontrol :food="foodsItem"></cartcontrol>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'
  import shopcart from '../shopcart/shopcart'
  import cartcontrol from '../cartcontrol/cartcontrol'
  // let wrapper = document.querySelector('.menu-wrapper')
  // let scroll = new BScroll(wrapper)

  const ERR_OK = 0;
  export default {
    // 操作数据，get、set数据
    props: {
      seller: {
        type: Object
      }
    },
    data(){
      return {
        goods: [],
        listHeight: [],
        scrollY: 0
      };
    },
    computed: {
      currentIndex(){
        for (let i = 0; i < this.listHeight.length; i++){
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)){
            return i;
          }
        }
        return 0;
      },
      selectFoods() {
        let foods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count){
              foods.push(food)
            }
          });
        });
        return foods;
      }
    },
    created(){
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
      this.$http.get("/api/goods").then(response => {
        response = response.body;
        if (response.errno === ERR_OK){
          this.goods = response.data;
          // 异步更新数据，数据更新并没有更新dom,dom高度没有发生变化，所以要在vue的接口nextTick的回调函数里执行滚动方法
          this.$nextTick(() => {
            this._initScroll();
            this._caculateHeight();
          })
        }
      })
    },
    methods: {
      selectMenu(index, event) {
        // 触发浏览器原生的点击事件的时候 ，此时event没有_constructed属性,返回false时直接返回，防止在pc端触发两次点击事件
        if (!event._constructed){
          return;
        }
        let foodList = this.$refs.foodsWrapper.getElementsByClassName("food-list-hook")
        let el = foodList[index]
        this.foodsScroll.scrollToElement(el,300);
      },
      _initScroll() {
        // 由 vue1.X的 v-el 更新成2.0版本的 ref：被用来给元素或子组件注册引用信息
        this.menuScroll = new BScroll(this.$refs.menuWrapper, {
          click: true
        });
        this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
          // 探针，实时监测滚动的位置
          // 当 probeType 为 3 的时候，不仅在屏幕滑动的过程中，而且在 momentum 滚动动画运行过程中实时派发 scroll 事件
          click: true,
          probeType: 3
        });
        // console.log(typeof this.foodsScroll) object
        // 监听scroll事件
        this.foodsScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y));
        })
      },
      _caculateHeight() {
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook')
        let height = 0;
        this.listHeight.push(height);
        for (let i = 0; i < foodList.length; i++){
          let item = foodList[i];
          height += item.clientHeight
          this.listHeight.push(height);
        }
      }
    },
    components: {
      shopcart,
      cartcontrol
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"

  .goods
    display: flex
    position: absolute
    top: 174px
    bottom: 46px
    width: 100%
    overflow: hidden
    .menu-wrapper
      flex: 0 0 80px
      background: #f3f5f7
      .goodsItem
        display:table
        padding: 0px 12px
        width: 56px
        height: 54px
        line-height: 14px
        &.current
          position: relative
          z-index: 10
          margin-top: -1px
          background: #fff
          font-weight: 700
          .text
            border-none()
        .icon
          display:inline-block
          vertical-align: top
          width: 12px
          height: 12px
          margin-right: 2px
          background-size: 12px 12px
          background-repeat: no-repeat
          &.decrease
            bg-image("decrease_3")
          &.discount
            bg-image("discount_3")
          &.guarantee
            bg-image("guarantee_3")
          &.invoice
            bg-image("invoice_3")
          &.special
            bg-image("special_3")
        .text
          display: table-cell
          width: 56px
          vertical-align : middle
          border-1px(rgba(7,17,27,0.1))
          font-size: 12px
    .foods-wrapper
      flex: 1
      .foodsType
        width: 100%
        .foods-type-name
          display: block
          padding-left: 12px
          height: 26px
          border-left: 2px solid #d9dde1
          background: #f3f5f7
          color: rgb(147,153,159)
          font-size: 12px
          line-height: 26px
        .foodsItem
          display: flex
          margin: 18px
          padding-bottom: 18px;
          border-1px(rgba(7,17,27,0.1))
          &:last-child
            border-none()
            margin-bottom: 0px;
          .icon
            flex: 0 0 57px
            padding-right: 10px
          .content
            flex: 1
            .name
              font-size: 14px
              color: rgb(7,17,27)
              height: 14px
              line-height:14px
              margin: 2px 0px 8px
            .description,.extra
              line-height: 10px
              color: rgb(147,153,159)
            .description
              font-size: 10px
              margin-bottom: 8px;
            .extra
              margin-top: 8px
              font-size: 10px
              .sellCount
                margin-right: 12px
            .price-box
              font-size: 10px
              font-weight: 700;
              line-height: 24px
              .price
                font-size: 14px
                color: rgb(240,20,20)
                margin-right: 8px
                &:before
                  content: '￥'
                  font-weight: normal;
              .oldPrice
                color: rgb(147,153,159)
                text-decoration:line-through
                &:before
                  content: '￥'
                  font-weight: normal;

          .cart-wrapper
            position: absolute
            right: 0px
            bottom: 12px
</style>
