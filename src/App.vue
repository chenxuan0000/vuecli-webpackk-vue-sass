<template>
<div>
  <v-head :seller="seller"></v-head>
  <div class="tab">
    <div class="tab-item">
      <router-link to="/goods">英雄</router-link>
    </div>
    <div class="tab-item">
      <router-link to="/ratings">评价</router-link>
    </div>
    <div class="tab-item">
      <router-link to="/seller">玩家</router-link>
    </div>
  </div>
  <router-view :seller="seller"></router-view>
</div>
</template>

<script>
import header from './components/header/header.vue';
const ERR_OK = 0;
export default {
  data() {
    return {
      seller: {}
    };
  },
  created() {
    // GET /someUrl
    this.$http.get('/api/seller').then(response => {
      // get body data
      response = response.body;
      if (response.errno === ERR_OK) {
        this.seller = response.seller;
      }

    }, response => {
      // error callback
    });
  },
  components: {
    'v-head': header
  } //不能直接用header因为header是h5的标签会冲突
};
</script>

<style lang="scss">
@import './common/scss/base';
.tab {
    display: flex;
    @include border-1px(rgba(7,17,27,.1));
    .tab-item {
        height: 40px;
        line-height: 40px;
        flex: 1;
        text-align: center;
        a {
            color: rgb(77, 85, 93);
            text-decoration: none;
            font-size: 14px;
            &.active {
                color: rgb(240, 20, 20);
            }
        }
    }
}
</style>
