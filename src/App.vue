<template>
  <div><canvas></canvas></div>
</template>

<script>
import * as PIXI from "pixi.js";

export default {
  data() {
    return {};
  },

  mounted() {
    const app = new PIXI.Application({
      //view: canvas,
      width: window.innerWidth, //canvas横幅
      height: window.innerHeight, //canvas縦幅
      backgroundColor: 0x000000, //背景色
      autoResize: true, //リサイズ処理
    });
    document.body.appendChild(app.view);

    app.stage.interactive = true;

    const container = new PIXI.Container();
    app.stage.addChild(container);

    const displacementSprite = PIXI.Sprite.from(
      "https://pixijs.io/pixi-filters/tools/screenshots/dist/original.png"
    );
    // Make sure the sprite is wrapping.
    displacementSprite.texture.baseTexture.wrapMode = PIXI.WRAP_MODES.REPEAT;
    const displacementFilter = new PIXI.filters.NoiseFilter(displacementSprite);
    displacementFilter.padding = 100;

    app.stage.addChild(displacementSprite);

    displacementSprite.filters = [displacementFilter];

    displacementFilter.noise = 0.5;
    displacementFilter.seed = 1;

    app.ticker.add(() => {
      displacementFilter.seed = Math.floor(Math.random() * 10);
    });
  },
};
</script>