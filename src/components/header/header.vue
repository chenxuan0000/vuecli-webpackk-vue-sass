<template>
<div class="header">
  <div class="content-wrapper">
    <div class="avater">
      <img height="64" width="80" :src="seller.avatar" alt="">
    </div>
    <div class="content">
      <p class="title"><span></span>{{seller.name}}</p>
      <p class="description">{{seller.description}}/{{seller.deliveryTime}}分钟左右</p>
      <p class="supports" v-if="seller.supports">
        <iconType :size="1" :type="seller.supports[0].type"></iconType>{{seller.supports[0].description}}
      </p>
    </div>
    <div class="supports-count" v-if="seller.supports" @click="showDetail">
      <span class="count">{{seller.supports.length}}个</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
  </div>
  <div class="bulletin-wrapper" @click="showDetail">
    <span class="image"></span>
    <span class="text">{{seller.bulletin}}</span>
    <i class="icon-keyboard_arrow_right"></i>
  </div>
  <div class="background">
    <img :src="seller.avatar" alt="" height="100%" width="100%">
  </div>
  <transition name="fade">
    <div class="detail-box" v-show="detailShow">
      <div class="main-wrapper clearfix">
        <div class="main">
          <h1 class="name">{{seller.name}}</h1>
          <div class="star-wrapper">
            <star :size="48" :score="seller.score"></star>
          </div>
          <div class="title">
            <div class="line"></div>
            <div class="text">最新消息</div>
            <div class="line"></div>
          </div>
          <ul v-if="seller.supports" class="supports">
            <li class="supports-item" v-for="item in seller.supports">
              <iconType :size="2" :type="item.type"></iconType>
              <span class="text">{{item.description}}</span>
            </li>
          </ul>
          <div class="title">
            <div class="line"></div>
            <div class="text">游戏公告</div>
            <div class="line"></div>
          </div>
          <p class="description">{{seller.bulletin}}</p>
        </div>
      </div>
      <div class="close" @click="hideDetail">
        <i class="icon-close"></i>
      </div>
    </div>
  </transition>
</div>
</template>

<script>
import star from '../star/star.vue';
import iconType from '../iconType/iconType.vue';
export default {
  props: {
    seller: {
      type: Object,
    }
  },
  data() {
    return {
      detailShow: false
    }
  },
  methods: {
    showDetail() {
      this.detailShow = true;
    },
    hideDetail() {
      this.detailShow = false;
    }
  },
  created() {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
  },
  components: {
    star,
    iconType
  }
}
</script>

<style lang="scss" scoped="" type="text/css">
@import '../../common/scss/base';
$fs20: 10px;
$fs24: 12px;
$fs32: 16px;
.header {
    position: relative;
    overflow: hidden;
    background-color: rgba(7,17,27,.5);
    color: #fff;
    .content-wrapper {
        position: relative;
        padding: 24px 12px 18px 14px;
        font-size: 0;
        > div {
            display: inline-block;
        }
        .supports-count {
            position: absolute;
            right: 12px;
            bottom: 14px;
            padding: 0 10px;
            height: 23px;
            line-height: 24px;
            border-radius: 24px;
            background-color: rgba(0,0,0,.2);
            font-size: $fs20;
            i {
                vertical-align: middle;
            }
        }
        .avater {
            vertical-align: sub;
            img {
                border-radius: 2px;
            }
        }
        .content {
            margin-left: 16px;
            .title {
                font-size: $fs32;
                font-weight: bold;
                margin: 2px 0 8px;
                > span {
                    display: inline-block;
                    margin-right: 6px;
                    vertical-align: sub;
                    width: 30px;
                    height: 18px;
                    @include bg-img( 'brand');
                    background-size: cover;
                    background-repeat: no-repeat;
                }
            }
            .description {
                margin: 8px 0 10px;
                font-size: $fs24;
            }
            .supports {
                width: 130px;
                overflow: hidden;
                white-space: nowrap;
                text-overflow: ellipsis;
                font-size: $fs20;
                line-height: 13px;
            }
        }
    }
    .bulletin-wrapper {
        position: relative;
        height: 28px;
        line-height: 28px;
        padding: 0 22px 0 12px;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
        background-color: rgba(7,17,27,.2);
        .image {
            display: inline-block;
            width: 22px;
            height: 12px;
            vertical-align: middle;
            @include bg-img( 'bulletin');
            background-size: cover;
            background-repeat: no-repeat;
        }
        .text {
            font-size: $fs20;
            margin: 0 3px;
        }
        i {
            position: absolute;
            font-size: $fs20;
            right: 10px;
            top: 11px;
        }
    }
    .background {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: -1;
        filter: blur(10px);
    }
    .detail-box {
        position: fixed;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        z-index: 100;
        overflow: auto;
        transition: all 0.4s;
        opacity: 1;
        background-color: rgba(7,17,27,.8);
        backdrop-filter:blur(10px); //ios背景色加模糊
        &.fade-enter-active,
        &.fade-leave-active {
            opacity: 0;
            background-color: rgba(7,17,27,0);
        }
        .main-wrapper {
            min-height: 100%;
            color: #fff;
            width: 100%;
            .main {
                width: 85%;
                margin-top: 65px;
                margin-left: auto;
                margin-right: auto;
                padding-bottom: 65px;
                .name {
                    font-weight: 700;
                    line-height: $fs32;
                    font-size: $fs32;
                    text-align: center;
                }
                .star-wrapper {
                    margin-top: 16px;
                    text-align: center;
                }
                .title {
                    display: flex;
                    margin: 28px auto 24px;
                    .line {
                        flex: 1;
                        position: relative;
                        top: -6px;
                        border-bottom: 1px solid rgba(225,225,225,.2);
                    }
                    .text {
                        padding: 0 12px;
                        font-size: 14px;
                        font-weight: 700;
                    }
                }
                .supports {
                    .supports-item {
                        padding: 0 12px;
                        margin-bottom: 12px;
                        font-size: 0;
                        &:last-child {
                            margin-bottom: 0；;
                        }
                        .text {
                            line-height: 16px;
                            font-size: 12px;
                        }
                    }
                }
                .description {
                    padding: 0 12px;
                    font-size: 12px;
                    line-height: 24px;
                }
            }
        }
        .close {
            position: relative;
            width: 32px;
            height: 32px;
            font-size: 32px;
            margin: -64px auto 0;
        }
    }
}
</style>
