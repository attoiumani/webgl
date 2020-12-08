<template>
  <div>
    <swiper :options="swiperOption">
      <swiper-slide>スライダー１</swiper-slide>
      <swiper-slide>スライダー２</swiper-slide>
      <swiper-slide>スライダー３</swiper-slide>
    </swiper>
    <div slot="pagination" class="swiper-pagination" />
    <div slot="button-prev" class="swiper-button-prev" />
    <div slot="button-next" class="swiper-button-next" />
  </div>
</template>


<script>
import Vue from "vue";
import Swiper from "swiper/swiper-bundle.esm";
import getAwesomeSwiper from "vue-awesome-swiper/dist/exporter";
Vue.use(getAwesomeSwiper(Swiper));
import "swiper/swiper-bundle.css";

export default {
  data() {
    return {
      swiperOption: {
        speed: 1000, //スライドの切り替わりスピード
        spaceBetween: 30, //各スライドの余白
        centeredSlides: true, //スライダーを真ん中に
        loop: true, //無限ループ
        autoplay: {
          //スライドの自動切り替え
          delay: 5000, //スライドの自動切り替えの秒数
          disableOnInteraction: false, //何らかのアクション後の自動切り替えを再開
        },
        pagination: {
          //ページネーション設定
          el: ".swiper-pagination",
          clickable: true,
        },
        navigation: {
          //ナビゲーション設定
          nextEl: ".swiper-button-next",
          prevEl: ".swiper-button-prev",
        },
      },
    };
  },
};
</script>

<style scoped>
</style>