<template>
  <div id="app">
    <router-view></router-view>
    <FooterGuide v-show="$route.meta.showFooter"/>
  </div>
</template>

<script>
import FooterGuide from "@/components/FooterGuide/FooterGuide";
import {reqGetUser} from "@/api";
import {mapActions} from "vuex";

export default {
  name: 'App',
  components: {
    FooterGuide
  },
  async mounted() {
    const res = await reqGetUser('admin')
    console.log(res)
    this.getAddress();
  },
  methods:{
    ...mapActions('addressAbout',['getAddress'])
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .app
    width 100%
    height 100%
    background #f5f5f5
</style>
