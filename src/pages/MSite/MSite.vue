<template>
  <section class="msite">
    <!--首页头部-->
    <HeaderTop :title="address.name">
      <template v-slot:left>
        <router-link class="header_search" to="/search">
          <i class="iconfont icon-sousuo"></i>
        </router-link>
      </template>
      <template  v-slot:right>
        <router-link :to="userInfo.login ? '/userinfo' : '/login'" class="header_login">
         <span class="header_login_text" v-if="!userInfo.login">登录|注册</span>
          <span class="header_login_text" v-else><i class="iconfont icon-person"></i></span>

        </router-link>
      </template>
    </HeaderTop>
    <!--首页导航-->
    <nav class="msite_nav">
      <div class="swiper-container">
        <div class="swiper-wrapper">
          <div class="swiper-slide" v-for="(categorys, index) in categorysArr" :key="index">
            <a href="javascript:" class="link_to_food" v-for="(category, index) in categorys" :key="index">
              <div class="food_container">
                <img :src="category.img">
              </div>
              <span>{{category.name}}</span>
            </a>
          </div>
        </div>
        <!-- Add Pagination -->
        <div class="swiper-pagination"></div>
      </div>
    </nav>
    <!--首页附近商家-->
    <div class="msite_shop_list">
      <div class="shop_header">
        <i class="iconfont icon-xuanxiang"></i>
        <span class="shop_header_title">附近商家</span>
      </div>
      <ShopList/>
    </div>
  </section>
</template>

<script>
import HeaderTop from "@/components/HeaderTop/HeaderTop";
import ShopList from "@/components/ShopList/ShopList";
// Import Swiper Vue.js components
import { Swiper, Pagination } from 'swiper';

// Import Swiper styles
import 'swiper/swiper-bundle.min.css';
import {mapState} from "vuex";
Swiper.use(Pagination)

export default {
  name: "MSite",
  mounted() {
    this.$store.dispatch('addressAbout/getCategorys')
    this.$store.dispatch('addressAbout/getShops')
  },
  components:{
    HeaderTop,
    ShopList
  },
  methods:{
  },
  computed:{
    ...mapState('addressAbout',["userInfo",'address',"categorys"]),
    //根据categorys一维数组生成一个2维数组 小数组中的元素个数最大是8
    categorysArr(){
      const {categorys} = this
      //准备空的2维数组
      const arr = []
      let minArr = []
      //遍历
      categorys.forEach(c =>{
        if(minArr.length === 8){
          minArr = []
        }
        if(minArr.length === 0){
          arr.push(minArr)
        }
        minArr.push(c)
      })
      console.log(arr)
      return arr;
    }
  },
  watch: {
    categorys(newValue,oldValue) {
      console.log('watch@@@',newValue,oldValue)
      // DOM 还没有更新
      this.$nextTick(function () { //完成界面更新后调用
        // DOM 现在更新了
        new Swiper('.swiper-container',{
          loop: true,
          pagination:{
            el: '.swiper-pagination',
          }
        })
      })
    }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "../../common/stylus/mixins.styl"
.msite  //首页
  width 100%
  .msite_nav
    bottom-border-1px(#e4e4e4)
    margin-top 45px
    height 200px
    background #fff
    .swiper-container
      width 100%
      height 100%
      .swiper-wrapper
        width 100%
        height 100%
        .swiper-slide
          display flex
          justify-content center
          align-items flex-start
          flex-wrap wrap
          .link_to_food
            width 25%
            .food_container
              display block
              width 100%
              text-align center
              padding-bottom 10px
              font-size 0
              img
                display inline-block
                width 50px
                height 50px
            span
              display block
              width 100%
              text-align center
              font-size 13px
              color #666
      .swiper-pagination
        >span.swiper-pagination-bullet-active
          background #02a774
  .msite_shop_list
    top-border-1px(#e4e4e4)
    margin-top 10px
    background #fff
    .shop_header
      padding 10px 10px 0
      .shop_icon
        margin-left 5px
        color #999
      .shop_header_title
        color #999
        font-size 14px
        line-height 20px
</style>