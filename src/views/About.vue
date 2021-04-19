
<template>
  <div class="main">
    <canvas id="canvas"></canvas>
    <div class="toriggermessi"></div>
    <div class="toriggerVVD"></div>
    <div class="toriggerCR7"></div>
  </div>
</template>

<script>
import { gsap /*Bounce*/ } from "gsap";
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

    const container = new PIXI.Container(); //container作成

    app.stage.addChild(container); //stageにcontainer追加

    /*Pixiフィルター*/
    const glitchFilter = new GlitchFilter({
      slices: 10,
      offset: 100,
      fillMode: 1,
      speed: 0,
    });
    /*Pixiフィルター*/

    /*テキスト変数*/

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

    /*テキスト変数*/

    /*Messi*/
    let number = new PIXI.Text("50 \n 51 \n 19", numberTextStyle);
    number.x = 300;
    number.y = 150;

    let text = new PIXI.Text("GAME\n\n\n\n\n  GOAL\n\n\n\n\n    assist", TextStyle);
    text.x = 650;
    text.y = 290;

    const image = PIXI.Texture.from(this.imgPath);
    const messigraph = new PIXI.Graphics();
    messigraph.beginTextureFill(
      image,
      0xffffff,
      1,
      new PIXI.Matrix(1, 0, 0, 1, 500, 50)
    );
    messigraph.x = 1200; //画像の位置
    messigraph.y = 100; //画像の位置
    messigraph.drawPolygon(-300, 560, 500, 560, 500, 50, -100, 50); // 頂点を配列で渡す [x1,y1,x2,y2,....]左下,右下,右上,左上,x=横y=縦
    messigraph.endFill();

  //ヒットエリア作成
    var poly = new PIXI.Polygon(
      new PIXI.Point(-300, 560),
      new PIXI.Point(500, 560),
      new PIXI.Point(500, 50),
      new PIXI.Point(-100, 50)
    );

    messigraph.hitArea = poly; //new PIXI.Rectangle(0, 0, 800, 512);
    messigraph.interactive = true;


    messigraph.on("mouseover", function () {
      //マウスホバー処理
      app.ticker.maxFPS = 5;
      app.ticker.add(function () {
        number.filters = [glitchFilter];
        messigraph.filters = [glitchFilter];
        gsap.to(glitchFilter, 0, {
          offset: Math.floor(Math.random() * 100),
          slices: Math.floor(Math.random() * 10),
        });
      });
    });

    messigraph.on("mouseout", function () {
      app.ticker.add(function () {
        glitchFilter.offset = 0;
        glitchFilter.slices = 0;
      });
    });

    const tlmessi = gsap.timeline({
      scrollTrigger: {
        trigger: ".toriggermessi", // 要素".b"がビューポートに入ったときにアニメーション開始
        start: "top center", // アニメーション開始位置
        end: "top 200px", // アニメーション終了位置
        //scrub: true, // アニメーションをスクロール位置にリンクさせる
        markers: true, // マーカー表示
      },
    });
    gsap.set(text, { alpha: 0 });
    tlmessi
      .to(text,9,{ alpha: 1 })
    /*Messi*/

    /*VVD*/
    const VVDimage = PIXI.Texture.from(this.imgPath1);

    let VVD = new PIXI.Text("Virgil van Dijk", TextStyle1);
    VVD.x = 900;
    VVD.y = 1100;

    let VVDtext = new PIXI.Text("CL：優勝", TextStyle1);
    VVDtext.x = 1000;
    VVDtext.y = 1300;

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

    const tlVVD = gsap.timeline({
      scrollTrigger: {
        trigger: ".toriggerVVD", // 要素".b"がビューポートに入ったときにアニメーション開始
        start: "top center", // アニメーション開始位置
        end: "top 200px", // アニメーション終了位置
        //scrub: true, // アニメーションをスクロール位置にリンクさせる
        markers: true, // マーカー表示
      },
    });
    gsap.set(VVDgraph, { alpha: 0.0 });
    gsap.set(VVD, { alpha: 0.0 });
    gsap.set(VVDtext, { alpha: 1.0 });
    tlVVD
      .to(VVDgraph, { alpha: 1 })
      .to(VVD, { alpha: 1 } /*"<"*/)
      .from(VVDtext, 4, { y: -10, alpha: 0 });
    /* VVD*/

    /*CR7 */
    const CR7image = PIXI.Texture.from(this.imgPath2);

    let CR7 = new PIXI.Text("Cristiano Ronaldo", TextStyle1);
    CR7.skew.set(0, 0);
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

    const tlcr7 = gsap.timeline({
      scrollTrigger: {
        trigger: ".toriggerCR7", // 要素".b"がビューポートに入ったときにアニメーション開始
        start: "top center", // アニメーション開始位置
        end: "top 200px", // アニメーション終了位置
        markers: true, // マーカー表示
      },
    });
    //gsap.set(CR7graph, { alpha: 0 });
    gsap.set(CR7, { alpha: 0 });
    gsap.set(CR7graph, { alpha: 0 });
    tlcr7
      //.from(CR7graph, 4, { ease: Bounce.easeOut, y: 40 })
      .to(CR7graph, { alpha: 1 })
      .to(CR7, { alpha: 1 });
    /*CR7 */

    container.addChild(
      messigraph,
      number,
      text,
      VVD,
      VVDgraph,
      VVDtext,
      CR7,
      CR7graph
    );
  },
};
</script>

<style scoped>
.toriggermessi{
  position: absolute;
  top: 300px;
}
.toriggerVVD {
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