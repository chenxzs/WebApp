/* eslint-disable */
<template>
  <div>
    <!-- <mt-swipe :auto="4000">
      <mt-swipe-item v-for="item in headers" :key="item.id">
        <img :src="item.src">
      </mt-swipe-item>
    </mt-swipe> -->
    <swiper :list="headers" :isFull="true"></swiper>


   <div class="nav">
    <div class="row">
      <router-link  class="box" to="/home/news">
        <span class="base-icon news-icon"></span>
        <span class="content">新闻资讯</span>
      </router-link >
      <router-link class="box" to="/home/pictures">
        <span class="base-icon pictures-icon"></span>
        <span class="content">图片分享</span>
      </router-link>
       <router-link class="box" to="/home/phones">
        <span class="base-icon phone-icon"></span>
        <span class="content">数码产品</span>
      </router-link>
     
    </div>
    <div class="row">
       <a class="box">
        <span class="base-icon snacks-icon"></span>
        <span class="content">零食</span>
      </a>
      <a class="box">
        <span class="base-icon shoes-icon"></span>
        <span class="content">鞋类</span>
      </a>
      <a class="box">
        <span class="base-icon ball-icon"></span>
        <span class="content">球类</span>
      </a>
     
    </div>
  </div>
  </div>
</template>

<script>
import axios from "axios";
import swiper from '../commom/swiper.vue'
export default {
  data() {
    return {
      headers: []
    };
  },
  components:{
   swiper
  },
  created() {
    this.getHeaderData();
  },
  methods: {
    getHeaderData() {
      var that = this;
      axios
        .get("api/header")
        .then(function(response) {
          that.headers = response.data.result;
        })
        .catch(function(error) {

           console.log(error);
        });
    }
  }
};
</script>
<style scoped lang="scss">

.nav {
  background-color: rgba(255, 81, 80, 0.8);
  .row {
    display: flex;
    .box {
      border: 1px solid white;
      width: 33.33%;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      padding: 5px;
      .base-icon {
        width: 48px;
        height: 48px;
        border-radius: 50%;
        background-color: blue;
      }
      .news-icon{
           background: url("../../assets/img/news.png") no-repeat 8px;
           background-color: rgba(5, 236, 36, 0.8);
      }
      .pictures-icon{
           background: url("../../assets/img/pictures.png") no-repeat 8px;
            background-color: rgba(5, 97, 236, 0.925);
      }
      .snacks-icon{
           background: url("../../assets/img/snacks.png") no-repeat 8px;
            background-color: rgba(236, 26, 156, 0.8);
      }
      .shoes-icon{
           background: url("../../assets/img/shoes.png") no-repeat 8px;
            background-color: rgba(14, 250, 117, 0.8);;
      }
      .ball-icon{
           background: url("../../assets/img/ball.png") no-repeat 8px;
            background-color: rgba(161, 209, 30, 0.8);
      }
       .phone-icon{
           background: url("../../assets/img/phone.png") no-repeat 8px;
            background-color: rgba(226, 12, 112, 0.8);
      }
      .content {
        color: white;
        font-family: "\5B8B\4F53";
        margin-top: 5px;
        font-size: 1.1rem;
      }
    }
  }
}
</style>

