<template>
  <div id="app">
    <Header :seller="seller"></Header>
    <div class="tab">
      <div class="tab-item"><router-link to="/goods" >商品</router-link></div>
      <div class="tab-item"><router-link to="/ratings" >评论</router-link></div>
      <div class="tab-item"><router-link to="/seller" >商家</router-link></div>
    </div>
    <div class="content clearfix">
      <keep-alive>
        <router-view :seller="seller"></router-view>
      </keep-alive>
    </div>

  </div>
</template>

<script>
import {urlParse} from '../static/util.js';
import Header from './components/Header/Header';

export default {
  name: 'App',
  components: {
    Header,
  },
  data() {
    return {
      seller: {
        id: (()=>{
          let queryParam = urlParse();
          console.log(queryParam);
          return queryParam.id
        })()
      },  // 接收ajax请求过来的数据   ---- seller 数据;
      allData: {}  // 接收ajax请求过来的数据   ---- 所有数据;
    }
  },
  created() {
    this.$axios.post('https://result.eolinker.com/ryfVuuNe56d9619232220f51d7a2f231f3df0a6a54029bf?uri=vue-ele',{
      id: this.seller.id
    })
    .then((response)=> {
      this.seller = response.data.seller;
      this.allData = response.data;
      console.log(this.allData);
    }).catch((err)=> {
      console.log(err);
    })
  }


}
</script>

<style lang="scss">

  @import "./scss/base.scss";

  #app {
    .tab {
      display: flex;
      width: 100%;
      height: 1rem;
      line-height: 1rem;
      border-bottom: 1px solid rgba(7, 17, 27,0.1);
      .tab-item {
        flex: 1;
        font-size: 14px;
        text-align: center;
        cursor: pointer;
        & > a {
          display: block;
          text-decoration: none;
          color: rgba(77,85,93,1);
          &.active {
            color:  rgba(240, 20, 20,1);
          }
        }
      }
    }

  }
</style>
