<template>
  <div>
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <div class="tab-item">
        <a v-link="{ path:'/goods' }">商品</a>
      </div>
      <div class="tab-item">
        <a v-link="{ path:'/ratings' }">评价</a>
      </div>
      <div class="tab-item">
        <a v-link="{ path:'/seller' }">商家</a>
      </div>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
import header from './components/header/header.vue';
import {urlParse} from 'common/js/util.js';
export default{
  data () {
    return {
      seller: {
        id: (() => {
          let queryParam = urlParse();
          console.log(queryParam);
          return queryParam.id;
        })()
      }
    };
  },
  created () {
    this.$http.get('/api/seller?id=' + this.seller.id).then((response) => {
      // console.log(response);
      response = response.body;
      const ERR_OK = 0;
      if (response.errno === ERR_OK) {
        this.seller = response.data;
        // console.log(this.seller);
      }
    });
  },
  components: {
    'v-header': header
  }
};
</script>

<style lang="css" scoped>
  .tab{ width:100%;height:40px;line-height:40px;text-align:center;display:flex;}
  .tab .tab-item{ flex:1;}
  .tab .tab-item a{ display: block;font-size: 14px;color:rgb(77,85,93); }
  .tab .tab-item a.v-link-active{ color:rgb(240,20,20); }
</style>
