<template>
  <div class="container">
    <div class="card">
      <swiper :list="pictures" :isFull="false"></swiper>
    </div>
    <div class="card">
      <h3 class="title">{{details.title}}</h3>
      <div class="body">
        <p>
          销售价：
          <span class="now">￥{{details.now}}</span>市场价:
          <span class="market">￥{{details.old}}</span>
        </p>
        <span class="purchase-num">购买数量:</span>
        <counter-box :max="details.quantity" @getcount="getCount"></counter-box>
        <p class="footer">
          <button class="btn btn-rightnow">立即购买</button>
          <button class="btn btn-num" @click="addCart">加入购物车</button>
        </p>
      </div>
    </div>
    <div class="card">
      <h3 class="title">商品参数</h3>
      <div class="body">
        <p>
          商品货号：
          <span>{{details.number}}</span>
        </p>
        <p>
          库存情况:
          <span>{{details.quantity}} 件</span>
        </p>
        <p>
          上架时间:
          <span>{{details.time}}</span>
        </p>

        <p class="footer"></p>
      </div>
    </div>
    <div class="card">
      <div class="body">
        <button class="btn btn-outline-blue">图文介绍</button>
        <button class="btn btn-outline-red">加载更多</button>
      </div>
    </div>
    <!-- <transition v-on:before-enter="beforeEnter" v-on:enter="enter" @after-enter="afterEnter" >
         <div class="ball" v-show="flag"></div>
    </transition> -->

    <div class="ball" id="ball" ref=ball></div>
  </div>
</template>

<script>
import axios from "axios";
import swiper from "../commom/swiper.vue";
import CounterBox from "../commom/CounterBox.vue";
export default {
  name: "phoneinfo",

  data() {
    return {
      pictures: [],
      id: this.$route.params.id,
      details: {},
      flag: false,
      count:1
    };
  },
  created() {
    this.getPhoneInfo();
    this.getPhoneDetails();
  },
 
  components: {
    swiper,
    CounterBox
  },
  methods: {
    getPhoneInfo() {
      let that = this;
      axios
        .get("api/pictures/picturepreview", {
          params: {
            typeid: 2,
            time: new Date().getTime()
          }
        })
        .then(function(response) {
          that.pictures = response.data;
        })
        .catch(function(error) {
          console.log(error);
        });
    },
  
    getPhoneDetails() {
      let that = this;
      axios
        .get("api/phones/details", {
          params: {
            id: that.id
          }
        })
        .then(function(response) {
        
          that.details = response.data;
        })
        .catch(function(error) {
          console.log(error);
        });
    },
    getCount(count){
      this.count=count;
      
    },
    addCart() {
    //  this.flag = !this.flag;
     // let ball = document.getElementById("ball");
     let  phoneifo={"id": this.id,"nums":this.count,"price":this.details.now,"selected":true};
   
     this.$store.commit("addToCart",phoneifo);
     let ball=this.$refs.ball;
     let cart = document.getElementById("cartnums").getBoundingClientRect();
     let scroll=document.documentElement.scrollTop||document.body.scrollTop;
    
      ball.style.display = "block";
      setTimeout(function() {
        ball.style.left = cart.left+'px';
        ball.style.top = (cart.top +scroll)+'px';   
       }, 100);
        setTimeout(function() {
         ball.style.display = "none";
        ball.style.left = "7.5rem";
        ball.style.top = "21.2rem";   
       }, 1200);
     }
     //,
    // beforeEnter(el) {
    //   el.style.transform = "translate(0,0)";
     
    // },
    // enter(el, done) {
    //   el.offsetWidth;
    //   el.style.transform = "translate(15rem,20rem)";
    //   el.style.transition = "all 10s ease";
     
    //   done();
    // },
    // afterEnter(el) {
    //   this.flag = !this.flag;
    // }
  }
};
</script>

<style lang='scss' scoped>
.container {
  padding: 2%;

  .card {
    cursor: pointer;
    margin: 0.6rem auto;
    border-radius: 0.2rem;
    box-shadow: 0 4px 8px 0 rgba(83, 78, 78, 0.2),
      0 6px 20px 0 rgba(177, 151, 151, 0.19);
    .title {
      border-bottom: 1px solid #cdcdd9;
      padding: 0.5rem 0;
    }
    .body {
      padding-bottom: 1rem;
      .purchase-num {
        font-size: 0.8rem;
        padding: 0 0.5rem 0.7rem;
      }
      .btn-outline-red {
        color: rgb(219, 58, 58);
        background-color: white;
        border: 1px solid rgb(219, 58, 58);

        margin: 0.5rem;
        color: rgb(219, 58, 58);
        width: 97%;
        padding: 0.5rem;
        font-size: 1rem;
        border-radius: 0.2rem;
      }
      .btn-outline-blue {
        color: #3385ff;
        background-color: white;
        border: 1px solid #3385ff;
        width: 97%;
        margin: 0.5rem;
        color: #3385ff;
        padding: 0.5rem;
        font-size: 1rem;
        border-radius: 0.2rem;
      }
      p {
        margin: 0.5rem 0;
        font-size: 0.8rem;
        color: #666;
        padding-left: 0.5rem;
        .now {
          font-size: 1rem;
          color: red;
          margin-right: 1rem;
        }
        .market {
          text-decoration: line-through;
        }
        .btn {
          padding: 0.3rem 0.5rem;
          color: white;
          border-radius: 0.2rem;
          margin-right: 1rem;
        }
        .btn-rightnow {
          background: #3385ff;
        }
        .btn-num {
          background: rgba(255, 81, 80, 0.8);
        }

        p:last-child {
          margin-bottom: 1rem;
        }
      }
    }
  }
  .ball {
    width: 1rem;
    height: 1rem;
    position: absolute;
    background: red;
    border-radius: 50%;
    left: 7.5rem;
    top: 21.2rem;
    z-index: 999;
    display: none;
     transition: all 1s ease-in;
  }

}
</style>
