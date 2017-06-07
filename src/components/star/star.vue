<template>
<div class="star" :class="starType">
  <span class="star-item" v-for="(itemClass,i) in itemClasses" :class="itemClass" :key="i"></span>
</div>
</template>

<script>
const LEN = 5;
const CLS_ON = 'on';
const CLS_OFF = 'off';
const CLS_HALF = 'half';

export default {
  props: {
    size: {
      type: Number
    },
    score: {
      type: Number
    }
  },
  computed: {
    starType() {
      return 'star-' + this.size;
    },
    itemClasses() {
      let res = [];
      let score = Math.floor(this.score * 2) / 2;
      let hasDecimal = score % 1 !== 0;
      let integer = Math.floor(score);
      for (let i = 0; i < integer; i++) {
        res.push(CLS_ON);
      }
      hasDecimal && res.push(CLS_HALF);
      while (res.length < LEN) {
        res.push(CLS_OFF);
      }
      return res;
    }
  }
}
</script>

<style lang="scss" scoped="" type="text/css">
@import '../../common/scss/base';
.star {
    font-size: 0;
    .star-item {
        display: inline-block;
        background-repeat: no-repeat;
        background-size: cover;
    }
    &.star-48 {
        .star-item {
            width: 20px;
            height: 20px;
            margin-right: 22px;
            &:last-child {
                margin-right: 0;
            }
            &.on {
                @include bg-img( 'star48_on');
            }
            &.half {
                @include bg-img( 'star48_half');
            }
            &.off {
                @include bg-img( 'star48_off');
            }
        }
    }
    &.star-36 {
        .star-item {
            width: 15px;
            height: 15px;
            margin-right: 6px;
            &:last-child {
                margin-right: 0;
            }
            &.on {
                @include bg-img( 'star36_on');
            }
            &.half {
                @include bg-img( 'star36_half');
            }
            &.off {
                @include bg-img( 'star36_off');
            }
        }
    }
    &.star-24 {
        .star-item {
            width: 10px;
            height: 10px;
            margin-right: 3px;
            &:last-child {
                margin-right: 0;
            }
            &.on {
                @include bg-img( 'star24_on');
            }
            &.half {
                @include bg-img( 'star24_half');
            }
            &.off {
                @include bg-img( 'star24_off');
            }
        }
    }
}
</style>
