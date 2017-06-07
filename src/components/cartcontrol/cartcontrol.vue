<template>
<div class="cartcontrol">
  <transition name="move">
    <div class="cart-decrease icon-remove_circle_outline" v-show="hero.count>0" @click="decreaseCart($event)"></div>
  </transition>
  <transition name="move">
    <div class="cart-count" v-show="hero.count>0">{{hero.count}}</div>
  </transition>
  <div class="cart-add icon-add_circle" @click="addCart($event)"></div>
</div>
</template>

<script>
import Vue from 'vue'
export default {
  props: {
    hero: {
      type: Object
    }
  },
  methods: {
    addCart(e) {
      if (!e._constructed) {
        return;
      }
      if (!this.hero.count) {
        Vue.set(this.hero, 'count', 1); //通过vue set才能使得新增的属性被watch到
        this.hero.count = 1;
      } else {
        this.hero.count += 1;
      }
      this.$emit('cartAdd',e.target);
    },
    decreaseCart(e) {
      if (!e._constructed) {
        return;
      }
      if (this.hero.count) {
        this.hero.count -= 1;
      }
    }
  }
}
</script>

<style lang="scss" scoped="" type="text/css">
.cartcontrol {
    font-size: 0;
    .cart-decrease {
        padding: 6px;
        display: inline-block;
        font-size: 24px;
        line-height: 24px;
        color: rgb(0,160,220);
        opacity: 1;
        transition: all .4s linear;
        // transform: translate3d(0,0,0) rotate(0);
        &.move-enter-active,
        &.move-leave-active {
            // opacity: 0;
            // transform: translate3d(25px,0,0) rotate(180deg);
        }
    }
    .cart-count {
        display: inline-block;
        vertical-align: top;
        min-width: 12px;
        line-height: 24px;
        padding-top: 6px;
        font-size: 12px;
        text-align: center;
        opacity: 1;
        transition: all .4s linear;
        color: rgb(147,153,59);
        &.move-enter-active,
        &.move-leave-active {
            opacity: 0;
        }
    }
    .cart-add {
        display: inline-block;
        padding: 6px;
        font-size: 24px;
        line-height: 24px;
        color: rgb(0,160,220);
    }
}
</style>
