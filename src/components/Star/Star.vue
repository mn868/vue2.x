<template>
  <div class="star" :class="'star-'+size">
    <span class="star-item" v-for="(sc, index) in starClasses" :class="sc" :key="index"></span>
  </div>
</template>

<script>
export default {
  name: "Star",
  props: {
    score: Number,
    size: Number
  },
  computed: {
    /*
    3.2 3 + 0 + 2
    3.5 3 + 1 + 1
     */
    starClasses () {
      const {score} = this
      const scs = []
        //向scs中添加n个'on'
      const scoreInteger = Math.floor(score)
      for (let i = 0; i < scoreInteger; i++) {
        scs.push('on')
      }
        //向scs中添加0/1个'half'
      if(score*10 - scoreInteger*10 >= 5){
        scs.push('half')
      }
        //向scs中添加n个'off'
      while (scs.length < 5){
        scs.push('off')
      }

      return scs
    }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus" scoped>
@import "../../common/stylus/mixins.styl"
.star //2x图 3x图
  float left
  font-size 0
  .star-item
    display inline-block
    background-repeat no-repeat
  &.star-48
    .star-item
      width 20px
      height 20px
      margin-right 22px
      background-size 20px 20px
      &:last-child
        margin-right: 0
      &.on
        bg-image('../../common/images/stars/star48_on')
      &.half
        bg-image('../../common/images/stars/star48_half')
      &.off
        bg-image('../../common/images/stars/star48_off')
  &.star-36
    .star-item
      width 15px
      height 15px
      margin-right 6px
      background-size 15px 15px
      &:last-child
        margin-right 0
      &.on
        bg-image('../../common/images/stars/star36_on')
      &.half
        bg-image('../../common/images/stars/star36_half')
      &.off
        bg-image('../../common/images/stars/star36_off')
  &.star-24
    .star-item
      width 10px
      height 10px
      margin-right 3px
      background-size 10px 10px
      &:last-child
        margin-right 0
      &.on
        bg-image('../../common/images/stars/star24_on')
      &.half
        bg-image('../../common/images/stars/star24_half')
      &.off
        bg-image('../../common/images/stars/star24_off')
</style>