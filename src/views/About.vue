<template>
  <canvas id="canvas"></canvas>
</template>

<script>
import * as PIXI from "pixi.js";
import { GlitchFilter } from "pixi-filters";

export default {
  data() {
    return {
      imgPath: require(`@/assets/cr7.jpg`),
    };
  },

  mounted() {
    const canvas = document.querySelector('canvas');

    const app = new PIXI.Application({
      view:canvas,
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

    const displacementSprite = PIXI.Sprite.from(this.imgPath);

    displacementSprite.filters = [glitchFilter];

    app.ticker.maxFPS = 1;
    app.ticker.add(function () {
      glitchFilter.offset = Math.floor(Math.random() * 100);
      glitchFilter.slices = Math.floor(Math.random() * 10);
    });

    app.stage.addChild(displacementSprite);
  },
};
</script>