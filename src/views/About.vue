<template>
  <div class="main">
    <h1>Lionel Messi</h1>
    <canvas id="canvas1"></canvas>
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
      imgPath1: require(`@/assets/logo.png`),
    };
  },

  mounted() {
    const app = new PIXI.Application({
      view: document.querySelector("#canvas1"),
      height: 9000,
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
    const image1 = PIXI.Texture.from(this.imgPath1);

    image.hitArea = new PIXI.Rectangle(0, 0, 800, 512);
    image.interactive = true;

    image.on("mouseover", function () {
      app.ticker.maxFPS = 5;
      app.ticker.add(function () {
        number.filters = [glitchFilter];
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

    let numberTextStyle = {
      fontSize: 200,
      fontWeight: "600",
      fill: 0x3b5a97,
      align: "center",
      fontStyle: "italic",
    };

    let TextStyle = {
      fill: 0x3b5a97,
      fontSize: 40,
      fontStyle: "oblique",
      fontWeight: "bold",
    };

    let number = new PIXI.Text("50 51 19", numberTextStyle);
    number.x = 0;
    number.y = 550;

    let text = new PIXI.Text(
      "GAME              GOAL                assist",
      TextStyle
    );
    text.x = 40;
    text.y = 760;

    const graphics = new PIXI.Graphics();
    graphics.beginTextureFill(
      image1
    );
    //graphics.pivot.x = 15;
    //graphics.pivot.y = 30;
    graphics.x = 100;
    graphics.y = 700;
    graphics.lineStyle(5, 0xffbd01, 1);
    graphics.drawCircle(200, 300, 100);
    graphics.endFill();

    container.addChild(image, number, graphics, text);
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