<template>
<span class="iconType" :class="[classMap[type], 'icon_'+size]"></span>
</template>

<script>
export default {
  props: {
    type: {
      type: Number
    },
    size: {
      type: Number
    }
  },
  created() {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
  },
}
</script>

<style lang="scss" scoped="" type="text/css">
@import '../../common/scss/base';
.iconType {
    display: inline-block;
    vertical-align: top;
    background-repeat: no-repeat;
    background-size: cover;
    &.icon_2 {
        width: 16px;
        height: 16px;
        margin-right: 6px;
        &.decrease {
            @include bg-img( 'decrease_2');
        }
        &.discount {
            @include bg-img( 'discount_2');
        }
        &.guarantee {
            @include bg-img( 'guarantee_2');
        }
        &.invoice {
            @include bg-img( 'invoice_2');
        }
        &.special {
            @include bg-img( 'special_2');
        }
    }
    &.icon_1 {
        width: 12px;
        height: 12px;
        margin-right: 3px;
        &.decrease {
            @include bg-img( 'decrease_1');
        }
        &.discount {
            @include bg-img( 'discount_1');
        }
        &.guarantee {
            @include bg-img( 'guarantee_1');
        }
        &.invoice {
            @include bg-img( 'invoice_1');
        }
        &.special {
            @include bg-img( 'special_1');
        }
    }
    &.icon_3 {
        width: 12px;
        height: 12px;
        margin-right: 2px;
        &.decrease {
            @include bg-img( 'decrease_3');
        }
        &.discount {
            @include bg-img( 'discount_3');
        }
        &.guarantee {
            @include bg-img( 'guarantee_3');
        }
        &.invoice {
            @include bg-img( 'invoice_3');
        }
        &.special {
            @include bg-img( 'special_3');
        }
    }

}
</style>
