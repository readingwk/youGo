<template>
  <div class="index-container">
    <div class="search-box">
      <input type="text" placeholder="搜索">
      <icon type="search" size="12"></icon>
    </div>
    <div class="swiper-container">
      <swiper indicator-dots autoplay circular>
        <block v-for="item in swiperList">
          <swiper-item>
            <img mode="aspectFill" :src="item.image_src">
          </swiper-item>
        </block>
      </swiper>
    </div>
    <div class="category-container">
      <div class="item" v-for="item in categoryList">
        <img :src="item.image_src" alt>
        <p>{{item.name}}</p>
      </div>
    </div>
    <div class="floor-container">
      <div class="floor" v-for="item in floorList">
        <div class="top">
          <img :src="item.floor_title.image_src" alt>
          <h3>{{item.floor_title.name}}</h3>
        </div>
        <div class="bottom">
          <img :src="it.image_src" alt v-for="(it, i) in item.product_list">
        </div>
      </div>
    </div>
    <div class="footer">
      <i class='iconfont icon-xiao'></i>我是有底线的
    </div>
    <div class="getTop" v-show="isShow" @click='backTop'>
      <i class='iconfont icon-jiantoushang'></i>
      顶部
    </div>
  </div>
</template>

<script>
import hxios from "../../utils/index.js";
export default {
  data() {
    return {
      swiperList: [],
      categoryList: [],
      floorList: [],
      isShow:false
    };
  },
  async created() {
    // let swiperRes = await hxios.get({
    //   url: "api/public/v1/home/swiperdata"
    // });
    // // console.log(res);
    // this.swiperList = swiperRes.data.message;
    // let categoryRes = await hxios.get({
    //   url: "api/public/v1/home/catitems"
    // });
    // //  console.log(categoryRes)
    // this.categoryList = categoryRes.data.message;

    // let floorRes = await hxios.get({
    //   url: "api/public/v1/home/floordata"
    // });
    // console.log(floorRes);
    // this.floorList = floorRes.data.message;
    let p1=hxios.get({
      url: "api/public/v1/home/swiperdata"
    });
    let p2=hxios.get({
      url: "api/public/v1/home/catitems"
    });
    let p3= hxios.get({
      url: "api/public/v1/home/floordata"
    });
    let totalRes=await Promise.all([p1,p2,p3]);
    this.swiperList = totalRes[0].data.message;
    this.categoryList = totalRes[1].data.message;
    this.floorList = totalRes[2].data.message;
  },
  onPageScroll(event){
    // console.log(event)
    if(event.scrollTop>170){
      this.isShow=true;
    }else{
      this.isShow=false;
    }
  },
  methods: {
    backTop(){
      wx.pageScrollTo({
        scrollTop: 0, //滚动到页面的目标位置（单位px）,
        duration: 300 //滚动动画的时长，默认300ms，单位 ms,
      });
    }
  },
};
</script>

<style lang='scss'>
$main: #ff2244;
.search-box {
  padding: 20rpx 16rpx;
  background-color: $main;
  position: fixed;
  box-sizing: border-box;
  width: 100%;
  z-index: 998;
  input {
    box-sizing: border-box;
    width: 100%;
    height: 60rpx;
    padding-left: 376rpx;
    background-color: #fff;
    font-size: 24rpx;
    border-radius: 10rpx;
  }
  icon {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }
}
.swiper-container {
  padding-top: 100rpx;
  swiper {
    swiper-item {
      width: 100%;
      height: 375rpx;
    }
    img {
      height: 375rpx;
      width: 100%;
      display: block;
    }
  }
}
.category-container {
  display: flex;
  padding-top: 24rpx;
  padding: 29rpx;
  // background-color: #fff;
  .item {
    flex: 1;
    img {
      display: block;
      width: 128rpx;
      height: 128rpx;
      margin: 0 auto;
      // padding-bottom: 15rpx;
    }
    p {
      text-align: center;
      font-size: 24rpx;
      margin-top: 10rpx;
    }
  }
}
.floor-container {
  .floor {
    .top {
      padding-top: 30rpx;
      padding-bottom: 30rpx;
      padding-left: 15rpx;
      position: relative;
      height: 90rpx;
      box-sizing: border-box;
      h3 {
        color: #ff7b94;
        position: absolute;
        left: 30rpx;
        top: 50%;
        transform: translateY(-50%);
      }
      img {
        position: absolute;
        height: 90rpx;
        left: 0;
        top: 0;
        display: block;
        width: 100%;
      }
    }
    .bottom {
      padding: 20rpx 0 0 16rpx;
      overflow: hidden;
      img {
        display: block;
        width: 33.33%;
        float: left;
        width: 230rpx;
        height: 190rpx;
        margin-right: 10rpx;
        &:first-child {
          width: 230rpx;
          height: 390rpx;
        }
        &:nth-child(2) {
          margin-bottom: 10rpx;
        }
        &:nth-child(3) {
          margin-bottom: 10rpx;
        }
      }
    }
  }
}
.footer {
  text-align: center;
  font-size: 28rpx;
  color: #aaa;
  display: flex;
  justify-content: center;
  margin-top: 15rpx;
  background-color: #f4f4f4;
  height: 180rpx;
  icon {
  }
}
.getTop{
  width: 90rpx;
  height: 90rpx;
  border-radius: 50%;
  background-color: #0094ff;
  color: #fff;
  text-align: center;
  position: fixed;
  right:10rpx;
  bottom:10rpx;
  font-size: 28rpx;
}
</style>
