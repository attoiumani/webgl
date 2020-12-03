<template>
  <div class="main">
    <h1>受賞</h1>
    <h2>Lionel Messi</h2>
    <canvas id="canvas1"></canvas>
    <div></div>
  </div>
</template>

<script>
import { gsap } from "gsap";
import * as PIXI from "pixi.js";
import { GlitchFilter } from "pixi-filters";

export default {
  data() {
    return {
      imgPath: require(`@/assets/images/2019/messi2019.jpg`),
    };
  },

  mounted() {
    const canvas = document.querySelector("#canvas1");

    const app = new PIXI.Application({
      view: canvas,
      width: window.innerWidth, //canvas横幅
      height: window.innerHeight, //canvas縦幅
    });


    const container = new PIXI.Container();
    app.stage.addChild(container);

    const glitchFilter = new GlitchFilter({
      slices: 10,
      offset: 100,
      fillMode: 1,
      speed: 0,
    });

    const image = PIXI.Sprite.from(this.imgPath);

    image.hitArea = new PIXI.Rectangle(0, 0, 800, 512);
    image.interactive = true;

    image.on("mouseover", function () {
      app.ticker.maxFPS = 5;
      app.ticker.add(function () {
        numbertext.filters = [glitchFilter];
        image.filters = [glitchFilter];
        gsap.to(glitchFilter, 0, {
          offset: Math.floor(Math.random() * 100),
          slices: Math.floor(Math.random() * 10),
        });
      });
    });

    image.on("mouseout", function () {
      app.ticker.add(function () {
        glitchFilter.offset = 0;
        glitchFilter.slices = 0;
      });
    });

    let goalnumber = new PIXI.Text("50", {
      fontSize: 80,
      fontWeight: "400",
      fill: 0x3b5a97,
      align: "center",
      fontStyle: "italic",
    });
    goalnumber.x = 50;
    goalnumber.y = 600;

    let text = new PIXI.Text("GAME             GOAL             ASSI", {
      fontSize: 40,
      fontWeight: "200",
      fill: "#f0f8ff",
      align: "center",
    });
    text.x = 160;
    text.y = 640;

    container.addChild(image, goalnumber, text);
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