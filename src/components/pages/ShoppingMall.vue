<template>
  <div>
    <div class="search-bar">
    <van-row class="test-row">
      <van-col span="3">
        <img :src="locationIcon" width="60%" class="location-icon"/>
      </van-col>
      <van-col span="16">
        <input type="text" class="search-input"/>
      </van-col>
      <van-col span="5">
        <van-button size="mini" class="search-button">查找</van-button>
      </van-col>
    </van-row>
    </div>
    <!--swiper area-->
    <div class="swiper-area">
      <van-swipe :autoplay="1000">
        <van-swipe-item v-for="( banner,index) in bannerPicArray" :key="index">
          <img v-lazy="banner.imageUrl" width="100%">
        </van-swipe-item>
      </van-swipe>
    </div>

    <!-- typebar -->
    <div class="type-bar">
      <div v-for="(cate,index) in category" :key="index">
        <img v-lazy="cate.image" width="90%">
        <span>{{cate.mallCategoryName}}</span>
      </div>
    </div>

<!-- adbanner area-->
<div>
  <img v-lazy='adBanner' width="100%"/>
</div>

<!-- 商品推荐 -->
<div class="recommend-area">
  <div class="recommend-title">
    商品推荐
  </div>
  <div class="recommend-body">
    <swiper :options="swiperOption">
      <swiper-slide v-for="(item,index) in recommendGoods" :key="index">
        <div class="recommend-item">
          <img :src="item.image" width="80%">
          <div>{{item.goodsName}}</div>
          <div>￥{{item.price}}(￥{{item.mallPrice  | moneyFilter}})</div>
        </div>
      </swiper-slide>
    </swiper>
  </div>
</div>
<!-- <swipe-default></swipe-default>
<swipe-default2></swipe-default2>
<swipe-default3></swipe-default3>
<swiper-text></swiper-text> -->

<div class="floor">
  <div class="recommend-title">
    {{floorName.floor1}}
  </div>
  <div class="floor_anomaly">
    <div class="floor-one">
      <img :src="floor1_0.image" width="100%">
    </div>
    <div>
      <div class="floor-two">
        <img :src="floor1_1.image" width="100%">
      </div>
       <div class="floor-two">
        <img :src="floor1_2.image" width="100%">
      </div>
    </div>
  </div>
  <div class="floor-rule">
    <div v-for="(item, index) in floor1.slice(3)" :key="index">
      <img :src="item.image" width="100%">
    </div>
  </div>
</div>
</div>
</template>

<script>
import axios from "axios";
import { swiper, swiperSlide } from "vue-awesome-swiper";
import "swiper/dist/css/swiper.css";
import swipeDefault from "../swiper/swiperDefault";
import swipeDefault2 from "../swiper/swiperDefault2";
import swipeDefault3 from "../swiper/swiperDefault3";
import swiperText from "../swiper/swiperText";
export default {
  data() {
    return {
      swiperOption: {
        slidesPerView: 3
      },
      msg: "ShoppingMall",
      locationIcon: require("../../assets/images/map.png"),
      bannerPicArray: [
        {
          imageUrl:
            "http://7xjyw1.com1.z0.glb.clouddn.com/simleVueDemoPic001.jpg"
        },
        {
          imageUrl:
            "http://7xjyw1.com1.z0.glb.clouddn.com/simleVueDemoPic002.jpg"
        },
        {
          imageUrl:
            "http://7xjyw1.com1.z0.glb.clouddn.com/simleVueDemoPic003.jpg"
        }
      ],
      category: [],
      adBanner: "",
      recommendGoods: [],
      floor1: [],
      floor1_0: {},
      floor1_1: {},
      floor1_2: {},
      floorName:[]
    };
  },
  components: {
    swiper,
    swiperSlide,
    swipeDefault,
    swipeDefault2,
    swipeDefault3,
    swiperText
  },
  created() {
    axios({
      url:
        "https://www.easy-mock.com/mock/5b0d55672179ff1c604e3bc8/smilevue/index",
      method: "get"
    })
      .then(response => {
        console.log(response);
        if (response.status == 200) {
          this.category = response.data.data.category;
          this.adBanner = response.data.data.advertesPicture.PICTURE_ADDRESS;
          this.recommendGoods = response.data.data.recommend;
          this.floor1 = response.data.data.floor1;
          this.floor1_0 = this.floor1[0];
          this.floor1_1 = this.floor1[1];
          this.floor1_2 = this.floor1[2];
          this.floorName = response.data.data.floorName;
        }
      })
      .catch(error => {
        console.log(error);
      });
  }
};
</script>

<style scoped>
.search-bar {
  height: 2.2rem;
  background-color: #e5017d;
  line-height: 2.2rem;
  overflow: hidden;
}
.search-input {
  width: 100%;
  height: 1.3rem;
  border-top: 0px;
  border-left: 0px;
  border-right: 0px;
  border-bottom: 1px solid #fff !important;
  background-color: #e5017d;
  color: #fff;
}
.location-icon {
  padding-top: 0.3rem;
  padding-left: 0.3rem;
}
.search-button {
  margin-left: 1rem;
}
.swiper-area {
  clear: both;
  max-height: 15rem;
  overflow: hidden;
}
.type-bar {
  background-color: #fff;
  margin: 0 0.3rem 0.3rem 0.3rem;
  border-radius: 0.3rem;
  font-size: 14px;
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
}
.type-bar div {
  padding: 0.3rem;
  font-size: 12px;
  text-align: center;
}
.recommend-area {
  background-color: #fff;
  margin-top: 0.3rem;
}
.recommend-title {
  border-bottom: 1px solid #eee;
  font-size: 14px;
  padding: 0.2rem;
  color: #e5017d;
}
.recommend-body {
  border-bottom: 1px solid #eee;
}
.recommend-item {
  width: 90%;
  border-right: 1px solid #eee;
  font-size: 12px;
  text-align: center;
}
.floor_anomaly{
  display: flex;
  flex-direction: row;
  background-color: #fff;
  border-bottom: 1px solid #ddd;
}
.floor_anomaly div{
  width: 10rem;
  box-sizing: border-box;
  -webkit-box-sizing: border-box;
}

.floor-one{
  border-right: 1px solid #ddd;
}
.floor-two{
  border-bottom: 1px solid #ddd;
}
.floor-rule{
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  background-color: #fff;

}
.floor-rule div{
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  width: 10rem;
  border-bottom: 1px solid #ddd;
}

.floor-rule div:nth-child(odd){
  border-right: 1px solid #ddd;
}
</style>
