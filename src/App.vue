<template>
  <div id="app">
    <nav-view></nav-view>
    <router-view></router-view>
    <div class="use-pc">
      <img src="/static/imgs/disable.svg" />
      <br>
      <p>{{"ui.usepc"|i18n}}</p>
    </div>
    <transition name="slide-fade">
    <div v-if="getMsg != null && getMsg.msg != null" class="msg">
      <p>{{ getMsg.msg|i18n }}</p>
    </div>
    </transition>
    <footer>
      <p>© Copyright 2017 catlib.io All Rights Reserved</p>
      <p>{{"ui.footer.description"|i18n}}</p>
    </footer>
  </div>
</template>

<script>
import '@/assets/stylus/base.styl'
import Nav from '@/components/modules/Nav'
export default {
  name: 'app',
  components: {
    'nav-view': Nav,
  },
  computed:{
    getMsg(){
      return this.$store.getters["env/msg"]
    }
  },
  mounted:function(){
    this.$store.commit('env/reClientId')
    this.$store.commit('console/loadCommandStack')

    var baseUrl = window.localStorage.getItem("baseUrl")
    if(baseUrl != null && baseUrl != ""){
      this.$store.commit('env/changeHost', baseUrl)
    }

    setInterval(() => {
      this.$store.dispatch('console/refresh')
    }, 1000)
  }
}
</script>

<style scoped lang="stylus">
@import "assets/stylus/_settings"
#app
  min-width 900px
footer
  padding 50px
  text-align center
  p
    padding 5px
.use-pc
  display none
.msg
  width 100%
  text-align center
  position fixed
  top 20px
  p
    margin 0 auto
    padding 5px 10px 5px 10px
    width 150px
    text-align center
    opacity 0.95
    border-radius 3px
    background-color $bg-color-v7
.slide-fade-enter-active
  transition all .3s ease
.slide-fade-leave-active
  transition all .3s ease
.slide-fade-enter, .slide-fade-leave-active
  transform translateY(-10px)
  opacity: 0;

@media screen and (max-width: 740px)
  .use-pc
    display block
    width 100%
    height 100%
    position fixed
    text-align center
    top 0
    left 0
    background-color $bg-color-v2
    img
      padding-top 10%
      width 128px
      height 128px
      margin 0 auto
    p
      margin-top 2rem
      display inline-block
      padding 0.25rem
      font-size 0.85rem
      background-color $bg-color-v7
      border-radius 5px
</style>