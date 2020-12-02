<template>
  <div class="main">
    <h1>受賞</h1>
    <h2>Lionel Messi</h2>
    <canvas id="canvas"></canvas>
    <div class="fontcolor-white fontsize-36">
      50<span class="fontsize-16">GAME</span> 51<span class="fontsize-16"
        >GOAL</span
      >
    </div>
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
      width: 800,
      height: 800,
    };
  },

  mounted() {
    const canvas = document.querySelector("canvas");

    const app = new PIXI.Application({
      view: canvas,
      width: this.width,
      height: this.height,
    });
    //document.body.appendChild(app.view);

    app.stage.interactive = true;

    const container = new PIXI.Container();
    app.stage.addChild(container);

    const glitchFilter = new GlitchFilter({
      slices: 10,
      offset: 100,
      fillMode: 1,
      speed: 0,
    });

    /*const glitchFilter1 = new GlitchFilter({
      slices: 10,
      offset: 100,
      fillMode: 1,
      speed: 0,
    });
*/

    const image = PIXI.Sprite.from(this.imgPath);

    image.hitArea = new PIXI.Rectangle(0, 0, this.width, 512);
    image.interactive = true;

    image.on("mouseover", function () {
      app.ticker.maxFPS = 5;
      app.ticker.add(function () {
        text.filters = [glitchFilter];
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

    let text = new PIXI.Text(50, {
      fontSize: 80,
      fontWeight: "400",
      fill: 0x3b5a97,
      align: "center",
    });
    text.x = 50;
    text.y = 600;

    app.stage.addChild(image);
    app.stage.addChild(text);
  },
};
</script>

<style scoped>
h1 {
  margin-top: 0px;
}

.main {
  font-size: 1.6rem;
}

.fontcolor-white {
  color: white;
}

.fontsize-16 {
  font-size: 1.6rem;
}

.fontsize-36 {
  font-size: 3.6rem;
}
</style>