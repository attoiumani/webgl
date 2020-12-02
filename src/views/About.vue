<template>
  <div>
    <h1>受賞</h1>
    <h2>Lionel Messi</h2>
    <canvas id="canvas"></canvas>
    <div>50試合出場 51goal</div>
  </div>
</template>

<script>
//import { gsap } from "gsap";
import * as PIXI from "pixi.js";
import { GlitchFilter } from "pixi-filters";

export default {
  data() {
    return {
      imgPath: require(`@/assets/images/2019/messi2019.jpg`),
      width:800,
      height:500
    };
  },

  mounted() {
    const canvas = document.querySelector("canvas");

    const app = new PIXI.Application({
      view: canvas,
      backgroundColor: 0x000000,
      width: this.width,
      height: this.height,
      autoResize: true,
    });

    app.stage.interactive = true;

    const container = new PIXI.Container();
    app.stage.addChild(container);

    const glitchFilter = new GlitchFilter({
      slices: 10,
      offset: 100,
      fillMode: 1,
      speed: 0,
    });

    const image = PIXI.Sprite.from(this.imgPath);

    image.hitArea = new PIXI.Rectangle(0, 0, this.width, this.height);
    image.interactive = true;

    image.on("mouseover", function () {
      app.ticker.maxFPS = 5;
      app.ticker.add(function () {
        image.filters = [glitchFilter];
        glitchFilter.offset = Math.floor(Math.random() * 100);
        glitchFilter.slices = Math.floor(Math.random() * 10);
      });
    });

    image.on("mouseout", function () {
      app.ticker.add(function () {
        glitchFilter.offset = 0;
        glitchFilter.slices = 0;
      });
    });

    app.stage.addChild(image);
  },
};
</script>