
<template>
  <div class="main">
    <canvas id="canvas"></canvas>
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
      imgPath2: require(`@/assets/images/2019/3.jpg`),
    };
  },

  mounted() {
    const app = new PIXI.Application({
      view: document.querySelector("#canvas"),
      width: document.body.clientWidth, //canvas横幅
      height: 2000, //canvas縦幅
      autoStart: true,
      autoResize: true,
      antialias: true,
      resolution: devicePixelRatio,
    });

    const container = new PIXI.Container();

    app.stage.addChild(container);

    const image = PIXI.Sprite.from(this.imgPath);
    const image1 = PIXI.Texture.from(this.imgPath1);
    const image2 = PIXI.Texture.from(this.imgPath2);

    const glitchFilter = new GlitchFilter({
      slices: 10,
      offset: 100,
      fillMode: 1,
      speed: 0,
    });

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

    let TextStyle1 = {
      fill: 0x3b5a97,
      fontSize: 60,
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

    let VVD = new PIXI.Text("Virgil van Dijk", TextStyle1);
    gsap.set(VVD, { alpha: 0.0 });
    gsap.to(VVD, 5, {
      alpha: 1.0,
      scrollTrigger: {
        trigger: ".torigger", // 要素".b"がビューポートに入ったときにアニメーション開始
        start: "top center", // アニメーション開始位置
        end: "top 200px", // アニメーション終了位置
        scrub: true, // アニメーションをスクロール位置にリンクさせる
        markers: true, // マーカー表示
      },
    });
    VVD.skew.set(0.0, 0.1);
    VVD.x = 900;
    VVD.y = 1100;

    const VVDimage = new PIXI.Graphics();
    VVDimage.beginTextureFill(
      image1,
      0xffffff,
      1,
      new PIXI.Matrix(1, 0, 0, 1, 500, 50)
    );
    VVDimage.x = 300;
    VVDimage.y = 1000;
    VVDimage.drawPolygon(-200, 650, 500, 635, 500, 50, -100, 50); // 頂点を配列で渡す [x1,y1,x2,y2,....]左下,右下,右上,左上,x=横y=縦
    VVDimage.endFill();
    gsap.set(VVDimage, { alpha: 0.0 });
    gsap.to(VVDimage, 5, {
      alpha: 1.0,
      scrollTrigger: {
        trigger: ".torigger", // 要素".b"がビューポートに入ったときにアニメーション開始
        start: "top center", // アニメーション開始位置
        end: "top 200px", // アニメーション終了位置
        scrub: true, // アニメーションをスクロール位置にリンクさせる
        markers: true, // マーカー表示
      },
    });

    const CR7image = new PIXI.Graphics();
    CR7image.beginTextureFill(
      image2,
      0xffffff,
      1,
      new PIXI.Matrix(1, 0, 0, 2, 500, 50)
    );
    CR7image.x = 800;
    CR7image.y = 1000;
    CR7image.drawPolygon(-200, 650, 500, 635, 500, 50, -100, 50); // 頂点を配列で渡す [x1,y1,x2,y2,....]左下,右下,右上,左上,x=横y=縦
    CR7image.endFill();

    container.addChild(image, number,text, VVD, VVDimage, CR7image);
  },
};
</script>

<style scoped>
.torigger {
  position: absolute;
  top: 1000px;
}
.canvas_wrapper {
  width: 1000px;
  margin: 0 auto;
}
</style>