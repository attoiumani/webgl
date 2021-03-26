
<template>
  <div class="main">
    <canvas id="canvas"></canvas>
    <div class="torigger"></div>
    <div class="toriggerCR7"></div>
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
      height: 3000, //canvas縦幅
      autoStart: true,
      autoResize: true,
      antialias: true,
      resolution: devicePixelRatio,
    });

    const container = new PIXI.Container();

    app.stage.addChild(container);

    const image = PIXI.Sprite.from(this.imgPath);
    const VVDimage = PIXI.Texture.from(this.imgPath1);
    const CR7image = PIXI.Texture.from(this.imgPath2);

    /*Pixiフィルター*/
    const glitchFilter = new GlitchFilter({
      slices: 10,
      offset: 100,
      fillMode: 1,
      speed: 0,
    });
    /*Pixiフィルター*/

    /*テキスト用変数*/

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

    /*ここまでテキスト変数*/

    /*Messi*/
    image.hitArea = new PIXI.Rectangle(0, 0, 800, 512);
    image.interactive = true;
    image.x = 500; //画像の位置

    image.on("mouseover", function () {
      //マウスホバー処理
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

    let number = new PIXI.Text("50 51 19", numberTextStyle);
    number.x = 500;
    number.y = 550;

    let text = new PIXI.Text(
      "GAME              GOAL                assist",
      TextStyle
    );
    text.x = 550;
    text.y = 760;
    /*Messi*/

    /*VVD*/
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

    const VVDgraph = new PIXI.Graphics();
    VVDgraph.beginTextureFill(
      VVDimage,
      0xffffff,
      1,
      new PIXI.Matrix(1, 0, 0, 1, 500, 50)
    );
    VVDgraph.x = 300;
    VVDgraph.y = 1000;
    VVDgraph.drawPolygon(-200, 650, 500, 635, 500, 50, -100, 50); // 頂点を配列で渡す [x1,y1,x2,y2,....]左下,右下,右上,左上,x=横y=縦
    VVDgraph.endFill();
    gsap.set(VVDgraph, { alpha: 0.0 });
    gsap.to(VVDgraph, 5, {
      alpha: 1.0,
      scrollTrigger: {
        trigger: ".torigger", // 要素".b"がビューポートに入ったときにアニメーション開始
        start: "top center", // アニメーション開始位置
        end: "top 200px", // アニメーション終了位置
        scrub: true, // アニメーションをスクロール位置にリンクさせる
        markers: true, // マーカー表示
      },
    });
    /* VVD*/

    /*CR7 */
    let CR7 = new PIXI.Text("Cristiano Ronaldo", TextStyle1);
    gsap.set(CR7, { alpha: 0.0 });
    gsap.to(CR7, 5, {
      alpha: 1.0,
      scrollTrigger: {
        trigger: ".toriggerCR7", // 要素".b"がビューポートに入ったときにアニメーション開始
        start: "top center", // アニメーション開始位置
        end: "top 200px", // アニメーション終了位置
        scrub: true, // アニメーションをスクロール位置にリンクさせる
        markers: true, // マーカー表示
      },
    });
    CR7.skew.set(2, 0);
    CR7.x = 400;
    CR7.y = 2000;

    const CR7graph = new PIXI.Graphics();
    CR7graph.beginTextureFill(
      CR7image,
      0xffffff,
      1,
      new PIXI.Matrix(1, 0, 0, 1, -30, 80)
    );
    CR7graph.x = 1000;
    CR7graph.y = 1900;
    CR7graph.drawPolygon(-30, 510, 800, 500, 800, 100, 20, 80); // 頂点を配列で渡す [x1,y1,x2,y2,....]左下,右下,右上,左上,x=横y=縦
    CR7graph.endFill();
    gsap.set(CR7graph, { alpha: 0.0 });
    gsap.to(CR7graph, 5, {
      alpha: 1.0,
      scrollTrigger: {
        trigger: ".toriggerCR7", // 要素".b"がビューポートに入ったときにアニメーション開始
        start: "top center", // アニメーション開始位置
        end: "top 200px", // アニメーション終了位置
        scrub: true, // アニメーションをスクロール位置にリンクさせる
        markers: true, // マーカー表示
      },
    });
    /*CR7 */

    container.addChild(image, number, text, VVD, VVDgraph, CR7, CR7graph);
  },
};
</script>

<style scoped>
.torigger {
  position: absolute;
  top: 1000px;
}
.toriggerCR7 {
  position: absolute;
  top: 2000px;
}
.canvas_wrapper {
  width: 1000px;
  margin: 0 auto;
}
</style>