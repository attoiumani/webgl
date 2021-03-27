<template>
  <div>
    <swiper class="swiper" :options="swiperOption">
      <swiper-slide class="swiper-slide"><router-link to="/about">2019</router-link></swiper-slide>
      <swiper-slide class="swiper-slide">スライダー２</swiper-slide>
      <swiper-slide class="swiper-slide">スライダー３</swiper-slide>
    </swiper>
    <div slot="pagination" class="swiper-pagination" />
    <div slot="button-prev" class="swiper-button-prev" />
    <div slot="button-next" class="swiper-button-next" />
  </div>
</template>


<script>
import Vue from "vue";
import SwiperCore, { EffectFade } from 'swiper';
import { Swiper, SwiperSlide } from "swiper/swiper-bundle.esm";
import getAwesomeSwiper from "vue-awesome-swiper/dist/exporter";
Vue.use(getAwesomeSwiper(Swiper, SwiperSlide));
import "swiper/swiper-bundle.css";
//import 'swiper/components/effect-fade/effect-fade.scss';

SwiperCore.use([EffectFade]);

export default {
  data() {
    return {
      swiperOption: {
        effect: 'coverflow',
        speed: 1000, //スライドの切り替わりスピード
        spaceBetween: 30, //各スライドの余白
        //centeredSlides: true, //スライダーを真ん中に
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
.swiper {
  width: 300px !important;
  height: 300px;
  position: absolute;
  left: 50%;
  top: 50%;
  margin-left: -150px;
  margin-top: -150px;
}

.swiper-slide {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  font-weight: bold;
  font-size: 32px;
  background-color: #2c8dfb;
  background-position: center;
  background-size: cover;
  color: white;
}

.swiper-pagination{
    left: 450px;
    top: 480px;
}
</style>