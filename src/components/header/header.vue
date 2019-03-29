<template>
  <!--用到的图片放在同一文件夹下，方便资源就近维护-->
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
          <img width="64" height="64" :src="seller.avatar">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <!--异步请求，seller初始化是空对象，seller.supports是undefined，不加v-if判断会报错-->
        <div v-if="seller.supports" class="supports">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" class="support-count" @click="showDetail">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="showDetail">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background-img">
      <img width="100%" height="100%" :src="seller.avatar">
    </div>
    <transition name="fade">
      <div v-show="detailShow" class="detail">
        <div class="detail-wrapper clearfix">
          <div class="detail-main">
            <div class="name">{{seller.name}}</div>
            <div class="star-wrapper">
              <star :size="48" :score="seller.score"></star>
            </div>
            <div class="title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <div v-if="seller.supports" class="supports">
              <ul>
                <li v-for="(item,index) in seller.supports" class="support-item" :key="index">
                  <span class="icon" :class="classMap[item.type]"></span>
                  <span class="text">{{item.description}}</span>
                </li>
              </ul>
            </div>
            <div class="title">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>
            </div>
            <div class="bulletin">
              <p>{{seller.bulletin}}</p>
            </div>
          </div>
        </div>
        <div class="detail-close">
          <i class="icon-close" @click="hideDetail"></i>
        </div>
      </div>
    </transition>
  </div>
</template>

<script type="text/ecmascript-6">
  import star from "../star/star";
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        detailShow: false
      };
    },
    methods: {
      showDetail() {
        this.detailShow = true; // detialShow变量依赖跟踪data方法里的detailShow,vue监测到变化在Dom上更新状态
      },
      hideDetail() {
        this.detailShow = false;
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    },
    components: {
      star
    }
  };

</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  .header
    position: relative
    overflow: hidden
    color: #fff
    background: rgba(7,17,27,0.5)
    .content-wrapper
      position: relative
      padding: 24px 12px 18px 24px
      font-size: 0  //去除标签折行产生的空格
      .avatar
        display:inline-block
        vertical-align: top
        img
          border-radius: 2px
      .content
        display:inline-block
        margin-left:16px
        .title
          margin: 2px 0 8px 0
          .brand
            display:inline-block
            vertical-align: top
            width:30px
            height:18px
            bg-image("brand")
            background-size: 30px 18px
          .name
            margin-left: 6px
            font-size: 18px
            font-weight: bold
            line-height: 18px

        .description
          margin-bottom: 10px
          line-height: 12px
          font-size: 12px
        .supports
          .icon
            display:inline-block
            vertical-align: top
            width: 12px
            height: 12px
            margin-right: 4px
            background-size: 12px 12px
            background-repeat: no-repeat
            &.decrease
              bg-image("decrease_1")
            &.discount
              bg-image("discount_1")
            &.guarantee
              bg-image("guarantee_1")
            &.invoice
              bg-image("invoice_1")
            &.special
              bg-image("special_1")
          .text
            line-height: 12px
            font-size: 10px

      .support-count
        position: absolute
        right: 12px
        bottom: 18px
        padding: 0px 8px
        height:24px
        line-height: 24px
        border-radius: 14px
        background: rgba(0,0,0,0.2)
        text-align： center
        .count
          font-size: 10px
        .icon-keyboard_arrow_right
          margin-left: 2px
          line-height: 12px
          font-size: 10px

    .bulletin-wrapper
      position: relative
      padding: 0px 22px 0px 12px
      height: 28px
      line-height: 28px
      white-space: nowrap
      overflow: hidden
      text-overflow: ellipsis
      background: rgba(7,17,27,0.2)
      .bulletin-title
        display:inline-block
        vertical-align: top
        margin-top: 7px
        width: 22px
        height: 12px
        bg-image("bulletin")
        background-repeat: no-repeat
        background-size: 22px 12px
      .bulletin-text
        vertical-align: top
        margin: 0px 4px
        font-size: 10px
      .icon-keyboard_arrow_right
        position: absolute
        top: 9px
        right: 12px
        font-size: 10px
    .background-img
      position: absolute
      top: 0
      left: 0
      width: 100%
      height: 100%
      z-index: -1
      filter: blur(10px)
    .detail
      position: fixed
      top: 0
      left: 0
      width: 100%
      height: 100%
      overflow: auto
      z-index: 100
      opacity: 1
      background: rgba(7, 17, 27, 0.8)
      backdrop-filter: blur(10  px)  //为一个元素后面区域添加图形效果（如模糊或颜色偏移）
      &.fade-enter-active, &.fade-leave-active
        transition: all 0.5s
      &.fade-enter, &.fade-leave-active
        opacity: 0
        background: rgba(7, 17, 27, 0)
      .detail-wrapper
        width: 100%
        min-height: 100%
        .detail-main
          margin-top: 64px
          padding-bottom: 64px
          .name
            line-height: 16px
            font-size: 16px
            font-weight:700
            text-align: center
          .star-wrapper
            margin-top: 18px
            padding: 2px 0
            text-align: center
          .title
            display: flex
            width: 80%
            margin: 28px auto 24px auto
            .line
              position: relative;
              top: -8px;
              flex: 1
              border-bottom: 1px solid rgba(255,255,255,0.2)
            .text
              font-size: 14px
              font-weight: 700
              padding: 0 12px
          .supports
            width: 80%
            margin: 0 auto
            ul
              padding: 0 12px
              .support-item
                margin-bottom: 12px
                font-size: 0
                &:last-child
                  margin-bottom: 0
                .icon
                  display:inline-block
                  vertical-align: top
                  width: 16px
                  height: 16px
                  margin-right: 6px
                  background-size: 16px 16px
                  background-repeat: no-repeat
                  &.decrease
                    bg-image("decrease_2")
                  &.discount
                    bg-image("discount_2")
                  &.guarantee
                    bg-image("guarantee_2")
                  &.invoice
                    bg-image("invoice_2")
                  &.special
                    bg-image("special_2")
                .text
                  line-height: 16px
                  font-size: 12px
          .bulletin
            width: 80%
            margin: 0px auto
            p
              padding: 0 12px
              font-size: 12px
              line-height: 24px
      .detail-close
        position: relative
        width: 32px
        height: 32px
        margin: -64px auto 0 auto
        font-size: 32px
        clear: both
        color: rgba(255,255,255,0.5)
</style>
