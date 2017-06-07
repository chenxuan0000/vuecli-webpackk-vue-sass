<template>
<div class="goods">
  <div class="menu-wrapper" ref="menuWrapper">
    <ul>
      <li v-for="(item,i) in goods" class="item" :class="{'cur':currentIndex === i}" @click="clickMenu(i,$event)">
        <span class="text">
            <iconType v-show="item.type>0" :size="3" :type="item.type"></iconType>{{item.name}}
          </span>
      </li>
    </ul>
  </div>
  <div class="hero-wrapper" ref="heroWrapper">
    <ul>
      <li v-for="item in goods" class="heroList heroList-hook">
        <h1 class="title">{{item.name}}</h1>
        <ul>
          <li v-for="hero in item.foods" class="heroItem">
            <div class="icon">
              <img width="57" height="57" :src="hero.icon" alt="">
            </div>
            <div class="content">
              <h2 class="name">{{hero.name}}</h2>
              <p class="desc">{{hero.description}}</p>
              <div class="extra">
                <span class="count">月售{{hero.sellCount}}份</span><span>好评率{{hero.rating}}%</span>
              </div>
              <div class="price">
                <span class="now">¥{{hero.price}}</span><span class="old" v-show="hero.oldPrice">¥{{hero.oldPrice}}</span>
              </div>
              <div class="cartcontrol-wrapper">
                <cartcontrol @cartAdd="_drop" :hero="hero"></cartcontrol>
              </div>
            </div>
          </li>
        </ul>
      </li>
    </ul>
  </div>
  <shopcart ref="shopcart" :selectHeros="selectHeros" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
</div>
</template>

<script>
import iconType from '../iconType/iconType.vue';
import shopcart from '../shopcart/shopcart.vue';
import cartcontrol from '../cartcontrol/cartcontrol.vue';
import BScroll from 'better-scroll';
const ERR_OK = 0;
export default {
  props: {
    seller: {
      type: Object
    }
  },
  data() {
    return {
      goods: [],
      listHeight: [],
      scrollY: 0
    }
  },
  computed: {
    currentIndex() {
      for (let i = 0; i < this.listHeight.length; i++) {
        let h1 = this.listHeight[i];
        let h2 = this.listHeight[i + 1];
        if (!h2 || this.scrollY >= h1 && this.scrollY < h2) {
          return i;
        }
      }
      return 0;
    },
    selectHeros() {
      let heros = [];
      this.goods.forEach((a) => {
        a.foods.forEach((b) => {
          if (b.count) {
            heros.push(b);
          }
        })
      });
      return heros;
    }
  },
  created() {
    this.$http.get('/api/goods').then((response) => {
      // get body data
      response = response.body;
      if (response.errno === ERR_OK) {
        this.goods = response.goods;
        this.$nextTick(() => {
          this._initScroll();
          this._calculateHeight();
        })
      }
    })
  },
  components: {
    iconType,
    shopcart,
    cartcontrol
  },
  methods: {
    _initScroll() {
      this.menuScroll = new BScroll(this.$refs.menuWrapper, {
        click: true //默认bscroll的prevrntdefault 事件阻止了click
      });
      this.heroScroll = new BScroll(this.$refs.heroWrapper, {
        probeType: 3, //检测实时监听位置
        click: true
      });
      this.heroScroll.on('scroll', (pos) => {
        this.scrollY = Math.abs(Math.round(pos.y));
      });
    },
    _calculateHeight() {
      let heroList = this.$refs.heroWrapper.getElementsByClassName('heroList-hook');
      let height = 0;
      this.listHeight.push(height);
      for (let i = 0; i < heroList.length; i++) {
        let hero = heroList[i];
        height += hero.clientHeight;
        this.listHeight.push(height);
      }
    },
    _drop(target) {
      //优化体验异步执行下落动画
      this.$nextTick(() => {
        this.$refs.shopcart.drop(target);
      });
    },
    clickMenu(index, e) {
      //判断是否是bscrol的排发的click事件
      if (!e._constructed) {
        return;
      }
      let heroList = this.$refs.heroWrapper.getElementsByClassName('heroList-hook');
      let el = heroList[index];
      this.heroScroll.scrollToElement(el, 350);
    }
  }
}
</script>

<style lang="scss" scoped="" type="text/css">
@import '../../common/scss/base';
.goods {
    position: absolute;
    display: flex;
    overflow: hidden;
    top: 176px;
    bottom: 46px;
    width: 100%;
    .menu-wrapper {
        flex: 0 0 80px;
        width: 80px; //兼容aroind 安卓浏览器
        background-color: #f3f5f7;
        .item {
            height: 54px;
            display: table;
            width: 56px;
            line-height: 14px;
            padding: 0 12px;
            &.cur {
                background-color: #fff;
                font-weight: 700;
                position: relative;
                margin-top: -1px;
                z-index: 12;
                .text {
                    @include border-none();
                }
            }
            .text {
                display: table-cell;
                vertical-align: middle;
                font-size: 12px;
                @include border-1px(rgba(7,17,27,.1));
            }
        }
    }
    .hero-wrapper {
        flex: 1;
        .title {
            padding-left: 14px;
            height: 26px;
            line-height: 26px;
            border-left: 2px solid #d9dde1;
            font-size: 12px;
            color: rgb(147,153,159);
            background-color: #f3f5f7;
        }
        .heroItem {
            display: flex;
            margin: 18px;
            padding-bottom: 18px;
            @include border-1px(rgba(7,17,27,.1));
            &:last-child {
                @include border-none();
                margin-bottom: 0;
            }
            .icon {
                flex: 0 0 57px;
                margin-right: 10px;
            }
            .content {
                flex: 1;
                .name {
                    margin: 2px 0 8px;
                    line-height: 16px;
                    font-size: 14px;
                    color: rgb(7,17,27);
                }
                .desc,
                .extra {
                    margin-bottom: 8px;
                    line-height: 12px;
                    font-size: 10px;
                    color: rgb(147,153,159);
                }
                .extra {
                    margin-bottom: 0;
                    line-height: 10px;
                    .count {
                        margin-right: 12px;
                    }
                }
                .price {
                    font-weight: 700;
                    line-height: 24px;
                    .now {
                        margin-right: 8px;
                        font-size: 14px;
                        color: rgb(240,20,20);
                    }
                    .old {
                        font-size: 10px;
                        color: rgb(147,153,159);
                        text-decoration: line-through;
                    }
                }
                .cartcontrol-wrapper {
                    position: absolute;
                    right: -8px;
                    bottom: 6px;
                }
            }
        }
    }
}
</style>
