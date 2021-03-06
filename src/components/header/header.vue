<template>
  <div class="header">
    <!-- 内容区 -->
    <div class="content-wrapper">
      <!-- 左侧头像 -->
      <div class="avatar-wrapper">
        <img v-bind:src="seller.avatar" class="avatar-img" alt="头像" width="64" height="64">
      </div>
      <!-- 右侧三行 -->
      <div class="content">
        <!-- 第一行 -->
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <!-- 第二行 -->
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <!-- 第三行 -->
        <div v-if="seller.supports" class="support">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <!--右下角的小按钮-->
      <div v-if="seller.supports" class="support-count" @click="showDetail">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <!-- 公告 -->
    <div class="bulletin-wrapper" @click="showDetail">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span><i
      class="icon-keyboard_arrow_right"></i>
    </div>
    <!-- 背景 -->
    <div class="background">
      <img :src="seller.avatar" alt="背景图" width="100%" height="100%">
    </div>
    <!-- 弹出层 stick footers -->
    <transition name="fade">
      <div class="detail" v-show="detailShow">
        <div class="detail-wrapper clearfix">
          <div class="detail-main">
            <h1 class="name">{{seller.name}}</h1>
            <div class="star-wrapper">
              <star :size="48" :score="seller.score"></star>
            </div>
            <!--第一个小标题-->
            <div class="title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <!--优惠信息列表-->
            <ul v-if="seller.supports" class="supports">
              <li class="support-item" v-for="(item,index) in seller.supports" :key="index">
                <span class="icon" :class="classMap[item.type]"></span>
                <span class="text">{{item.description}}</span>
              </li>
            </ul>
            <!--第二个小标题-->
            <div class="title">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>
            </div>
            <!--商家公告详情-->
            <div class="bulletin">
              <p class="content">{{seller.bulletin}}</p>
            </div>
          </div>
        </div>
        <div class="detail-close" @click="closeDetail">
          <i class="icon-close"></i>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import star from '../star/star'

export default {
  name: 'v-header',
  components: {
    star
  },
  data() {
    return {
      detailShow: false
    }
  },
  methods: {
    showDetail() {
      this.detailShow = !this.detailShow
    },
    closeDetail() {
      this.detailShow = !this.detailShow
    }
  },
  props: {
    // 接收由父组件传递过来的seller对象
    seller: {
      type: Object
    }
  },
  created() {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus" scoped>
  @import '../../common/stylus/mixin';

  .header
    color #FFF
    background-color rgba(7, 17, 27, 0.3)
    overflow hidden
    position relative
    .content-wrapper
      padding 24px 12px 18px 24px
      font-size 0
      position relative
      .avatar-wrapper
        display inline-block
        vertical-align top
        .avatar-img
          border-radius 2px
      .content
        display inline-block
        vertical-align top
        font-size 14px
        margin-left 16px
        .title
          margin 2px 0 8px 0
          .brand
            vertical-align top
            width 30px
            height 18px
            display inline-block
            bg-image('brand')
            background-size 30px 18px
            background-repeat no-repeat
          .name
            margin-left 6px
            font-size 16px
            font-weight 700
            line-height 18px
        .description
          margin-bottom 10px
          line-height 12px
          font-size 12px
        .support
          .icon
            vertical-align top
            display inline-block
            height 12px
            width 12px
            margin-right 4px
            margin-top 2px
            background-size 12px 12px
            background-repeat no-repeat
            &.decrease
              bg-image('decrease_1')
            &.discount
              bg-image('discount_1')
            &.guarantee
              bg-image('guarantee_1')
            &.invoice
              bg-image('invoice_1')
            &.special
              bg-image('special_1')
          .text
            line-height 12px
            font-size 12px
      .support-count
        position absolute
        right 12px
        bottom 18px
        color #FFF
        height 24px
        font-size 10px
        line-height 24px
        padding: 0 8px
        border-radius 14px
        background-color rgba(0, 0, 0, 0.2)
        text-align center
        .count
          font-size 10px
          vertical-align top
        .icon-keyboard_arrow_right
          font-size 10px
          line-height 24px
          margin-left 2px
    .bulletin-wrapper
      display flex
      flex-direction row
      align-items center
      flex-wrap nowrap
      height 28px
      width 100%
      font-size 12px
      background-color rgba(7, 17, 27, 0.2)
      .bulletin-title
        display inline-block
        width 22px
        height 12px
        margin-left 16px
        background-size 22px 12px
        background-repeat no-repeat
        bg-image('bulletin')
      .bulletin-text
        flex 1
        padding 0 4px
        white-space nowrap
        overflow hidden
        text-overflow ellipsis
      .icon-keyboard_arrow_right
        margin 2px 12px 0 0
    .background
      position absolute
      top 0
      left 0
      width 100%
      height 100%
      z-index -1
      filter blur(10px)
    .detail
      position fixed
      z-index 100
      height 100%
      width 100%
      overflow auto
      top 0
      left 0
      backdrop-filter blur(10px)
      background-color rgba(7, 17, 27, 0.8)
      &.fade-enter-active, &.fade-leave-active
        transition: opacity .5s
      &.fade-enter, &.fade-leave-to
        opacity: 0
      .detail-wrapper
        min-height 100%
        width 100%
        .detail-main
          margin-top 64px
          padding-bottom 64px
          .name
            font-size 16px
            text-align center
            line-height 16px
            font-weight 700
          .star-wrapper
            margin-top 18px
            padding 2px 0
            text-align center
          .title
            display flex
            flex-direction row
            width 80%
            margin 28px auto 24px auto
            .line
              flex 1
              position relative
              top: -6px
              border-bottom 1px solid rgba(255, 255, 255, 0.2)
            .text
              padding 0 12px
              font-size 14px
              font-weight 700
          .supports
            width 80%
            margin 0 auto
            .support-item
              padding 0 12px
              margin-bottom 12px
              font-size 0
              &:last-child
                margin-bottom 0
              .icon
                display inline-block
                width 16px
                height 16px
                vertical-align top
                margin-right 16px
                background-size 16px 16px
                background-repeat no-repeat
                &.decrease
                  bg-image('decrease_2')
                &.discount
                  bg-image('discount_2')
                &.guarantee
                  bg-image('guarantee_2')
                &.invoice
                  bg-image('invoice_2')
                &.special
                  bg-image('special_2')
              .text
                line-height 16px
                font-size 12px
          .bulletin
            width 80%
            margin 0 auto
            .content
              padding 0 12px
              line-height 24px
              font-size 12px
      .detail-close
        position relative
        width 32px
        height 32px
        margin -64px auto 0 auto
        clear both
        font-size 32px
</style>
