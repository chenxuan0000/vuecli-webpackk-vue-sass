<template>
<div class="shopcart">
  <div class="content">
    <div class="content-left">
      <div class="logo-wrapper">
        <div class="logo" :class="{'highlight':totalCount>0}">
          <i class="icon-shopping_cart" :class="{'highlight':totalCount>0}"></i>
        </div>
        <div class="num" v-show="totalCount>0">{{totalCount}}</div>
      </div>
      <div class="price" :class="{'highlight':totalCount>0}">¥{{totalPrice}}</div>
      <div class="desc">配送费¥{{deliveryPrice}}元</div>
    </div>
    <div class="content-right">
      <div class="pay" :class="payClass">{{payPrice}}</div>
    </div>
  </div>
  <div class="ball-container">
    <div v-for="ball in balls">
      <transition name="drop" @before-enter="beforeDrop" @enter="dropping" @after-enter="afterDrop">
        <div class="ball" v-show="ball.show">
          <div class="inner inner-hook"></div>
        </div>
      </transition>
    </div>
  </div>
</div>
</template>

<script>
export default {
  props: {
    selectHeros: {
      type: Array,
      default () {
        return [{
          price: 11,
          count: 1
        }];
      }
    },
    deliveryPrice: {
      type: Number,
      default: 0
    },
    minPrice: {
      type: Number,
      default: 0
    },
  },
  data() {
    return {
      balls: [{
        show: false
      }, {
        show: false
      }, {
        show: false
      }, {
        show: false
      }, {
        show: false
      }],
      dropBall: []
    }
  },
  methods: {
    drop(el) {
      for (let i = 0; i < this.balls.length; i++) {
        let ball = this.balls[i];
        if (!ball.show) {
          ball.show = true;
          ball.el = el;
          this.dropBall.push(ball);
          return;
        }
      }
    },
    beforeDrop: function(el) { //这个方法的执行是因为这是一个vue的监听事件
      let count = this.balls.length;
      while (count--) {
        let ball = this.balls[count];
        if (ball.show) {
          let rect = ball.el.getBoundingClientRect(); //获取小球的相对于视口的位移(小球高度
          let x = rect.left - 32;
          let y = -(window.innerHeight - rect.top - 22);
          el.style.display = 'none';
          el.style.webkitTransform = `translate3d(0,${y}px,0)`;
          el.style.transform = `translate3d(0,${y}px,0)`;
          let inner = el.querySelector('.inner-hook');
          inner.style.webkitTransform = `translate3d(${x}px,0,0)`;
          inner.style.transform = `translate3d(${x}px,0,0)`;
        }
      }
    },
    dropping: function(el, done) {
      let rf = el.offsetHeight; //触发重绘html
      this.$nextTick(() => {
        el.style.webkitTransform = 'translate3d(0, 0, 0)';
        el.style.transform = 'translate3d(0, 0, 0)';
        let inner = el.querySelector('.inner-hook');
        inner.style.webkitTransform = 'translate3d(0, 0, 0)';
        inner.style.transform = 'translate3d(0, 0, 0)';
        el.addEventListener('transitionend', done); //Vue为了知道过渡的完成，必须设置相应的事件监听器。
      })

    },
    afterDrop: function(el) {
      let ball = this.dropBall.shift();
      if (ball) {
        ball.show = false;
        el.style.display = 'none';
      }
    }
  },
  computed: {
    totalPrice() {
      let total = 0;
      this.selectHeros.forEach((hero) => {
        total += hero.price * hero.count
      })
      return total;
    },
    totalCount() {
      let count = 0;
      this.selectHeros.forEach((hero) => {
        count += hero.count
      })
      return count;
    },
    payPrice() {
      if (this.totalPrice === 0) {
        return `¥${this.minPrice}元起送`;
      } else if (this.totalPrice < this.minPrice) {
        let diff = this.minPrice - this.totalPrice;
        return `还差¥${diff}元起送`;
      } else {
        return '去结算';
      }
    },
    payClass() {
      if (this.totalPrice > this.minPrice) {
        return 'enough';
      } else {
        return 'not-enough';
      }
    }
  }
}
</script>

<style lang="scss" scoped="" type="text/css">
.shopcart {
    position: fixed;
    bottom: 0;
    left: 0;
    height: 48px;
    width: 100%;
    z-index: 20px;
    .content {
        display: flex;
        font-size: 0;
        color: rgba(255,255,255,.4);
        background-color: #141d27;
        .content-left {
            flex: 1;
            .logo-wrapper {
                display: inline-block;
                position: relative;
                top: -10px;
                margin: 0 10px;
                padding: 6px;
                width: 56px;
                height: 56px;
                box-sizing: border-box;
                vertical-align: top;
                border-radius: 50%;
                background-color: #141d27;
                .num {
                    position: absolute;
                    top: 0;
                    right: 0;
                    width: 24px;
                    height: 16px;
                    background-color: rgb(240,20,20);
                    border-radius: 16px;
                    line-height: 16px;
                    text-align: center;
                    font-size: 9px;
                    font-weight: 700;
                    color: #fff;
                    // h-length, v-length, blur, spread, colour
                    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, .4);
                }
                .logo {
                    width: 100%;
                    height: 100%;
                    border-radius: 50%;
                    text-align: center;
                    background-color: #2b343c;
                    &.highlight {
                        background-color: rgb(0,160,220);
                    }
                    .icon-shopping_cart {
                        line-height: 44px;
                        font-size: 24px;
                        color: #80858a;
                        &.highlight {
                            color: #fff;
                        }
                    }
                }
            }
            .price {
                display: inline-block;
                vertical-align: top;
                line-height: 24px;
                font-weight: 700;
                margin-top: 12px;
                padding-right: 10px;
                box-sizing: border-box;
                border-right: 1px solid rgba(255,255,255,.1);
                font-size: 16px;
                &.highlight {
                    color: #fff;
                }
            }
            .desc {
                display: inline-block;
                vertical-align: top;
                line-height: 24px;
                padding-left: 10px;
                margin-top: 12px;
                font-size: 11px;
                width: 78px;
                overflow: hidden;

            }
        }
        .content-right {
            flex: 0 0 98px;
            width: 98px;
            .pay {
                height: 48px;
                line-height: 50px;
                text-align: center;
                font-size: 12px;
                font-weight: 700;
                &.not-enough {
                    background-color: #2b333b;
                }
                &.enough {
                    background-color: #00b43c;
                    color: #fff;
                }
            }
        }
    }
    .ball-container {
        .ball {
            position: fixed;
            left: 32px;
            bottom: 22px;
            z-index: 200;
            transition: all 0.4s cubic-bezier(0.49,-0.29,0.75,0.41);
            .inner {
                width: 16px;
                height: 16px;
                transition: all 0.4s linear;
                border-radius: 50%;
                background-color: rgb(0,160,220);
            }
        }
    }
}
</style>
