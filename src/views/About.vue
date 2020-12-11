<template>
  <div class="main">
    <h1>Lionel Messi</h1>
    <canvas id="canvas1"></canvas>
    <div class="torigger"></div>
  </div>
</template>

<script>
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import * as PIXI from "pixi.js";
import { GlitchFilter } from "pixi-filters";

gsap.registerPlugin(ScrollTrigger);

export default {
  data() {
    return {
      imgPath: require(`@/assets/images/2019/messi2019.jpg`),
      imgPath1: require(`@/assets/images/2019/2.jpg`),
    };
  },

  mounted() {
    const app = new PIXI.Application({
      view: document.querySelector("#canvas1"),
      width: window.innerWidth, //canvas横幅
      height: 3000, //canvas縦幅
      /*width: 1000, //canvas横幅
      height: 4000, //canvas縦幅*/
      autoResize: true, //リサイズ処理
    });

    const container = new PIXI.Container();

    app.stage.addChild(container);

    const glitchFilter = new GlitchFilter({
      slices: 10,
      offset: 100,
      fillMode: 1,
      speed: 0,
    });

    const glitchFilter1 = new GlitchFilter({
      slices: 10,
      offset: 100,
      fillMode: 1,
      speed: 0,
    });

    const image = PIXI.Sprite.from(this.imgPath);
    const image1 = PIXI.Texture.from(this.imgPath1);

    image.hitArea = new PIXI.Rectangle(0, 0, 800, 512);
    image.interactive = true;
    image.x = 500;

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
    number.x = 500;
    number.y = 550;

    let text = new PIXI.Text(
      "GAME              GOAL                assist",
      TextStyle
    );
    text.x = 550;
    text.y = 760;

    let text2 = new PIXI.Text("Virgil van Dijk", TextStyle);
    gsap.set(text2, { alpha: 0.0 });
    gsap.to(text2, 5, {
      alpha: 1.0,
      scrollTrigger: {
        trigger: ".torigger", // 要素".b"がビューポートに入ったときにアニメーション開始
        start: "top center", // アニメーション開始位置
        end: "top 200px", // アニメーション終了位置
        //scrub: true, // アニメーションをスクロール位置にリンクさせる
        markers: true, // マーカー表示
      },
    });
    text2.x = 700;
    text2.y = 900;

    const graphics = new PIXI.Graphics();
    graphics.beginTextureFill(
      image1,
      0xffffff,
      1,
      new PIXI.Matrix(1, 0, 0, 1, 500, 50)
    );
    graphics.x = 300;
    graphics.y = 800;
    graphics.lineStyle(3, 0, 1);
    graphics.drawCircle(50, 300, 250);
    graphics.endFill();
    app.ticker.add(function () {
      gsap.to(graphics, 0, {
        filters : [glitchFilter1],
        offset: Math.floor(Math.random() * 100),
        slices: Math.floor(Math.random() * 10),
        scrollTrigger: {
          trigger: ".torigger", // 要素".b"がビューポートに入ったときにアニメーション開始
          start: "top center", // アニメーション開始位置
          end: "top 200px", // アニメーション終了位置
          //scrub: true, // アニメーションをスクロール位置にリンクさせる
          markers: true, // マーカー表示
        },
      });
    });
    container.addChild(image, number, graphics, text, text2);
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

.torigger {
  position: absolute;
  top: 1000px;
}
</style>