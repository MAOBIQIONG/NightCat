<template>
  <div>
    <div class="swipe-box">
      <div class="back" @click="goback"></div>
      <div class="title">{{demo06_index+1}}/{{demo06_list.length}}</div>
    </div>
    <swiper height="8rem" class="swiper-self" :list="demo06_list" :index="demo06_index" @on-index-change="demo06_onIndexChange" :show-dots="false"></swiper>
  </div>
</template>

<script>
  import xheader from '../../components/header/xheader.vue'
  import { Swiper, SwiperItem } from 'vux'
  const baseList = [{
    url: 'javascript:',
    img: 'https://static.vux.li/demo/1.jpg',
  }, {
    url: 'javascript:',
    img: 'https://static.vux.li/demo/2.jpg',
  }, {
    url: 'javascript:',
    img: 'https://static.vux.li/demo/5.jpg',
    fallbackImg: 'https://static.vux.li/demo/3.jpg'
  }]
  const urlList = baseList.map((item, index) => ({
    url: 'http://m.baidu.com',
    img: item.img,
    fallbackImg: item.fallbackImg,

  }))
  export default {
    components: {
      Swiper,
      SwiperItem,
      xheader
    },
    ready () {
    },
    methods: {
      goback () {
        this.$router.goBack()
      },
      demo06_onIndexChange (index) {
        this.demo06_index = index
      }
    },
    data () {
      return {
        demo06_list: urlList,
        demo06_index: 0,
        title: ''
      }
    }
  }
</script>

<style scoped>
  .swipe-box{
    display:-webkit-box;
    font-size: 1rem;
  }
  .swipe-box .title{
    -webkit-box-flex:1;
    color: #ccc !important;
    text-align: right;
    padding: .5rem .2rem;
  }
  .back{
    height: 2rem;
    width: .8rem;
    padding-left: .2rem;
    background: url('/static/images/goback.png') center center no-repeat;
    background-size: .8rem .8rem;
  }
</style>
