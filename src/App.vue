<template>
    <canvas id ="canvas"></canvas>
</template>

<script>
import * as PIXI from "pixi.js";


export default {
  data() {
    return {};
  },

  mounted() {
const app = new PIXI.Application();
document.body.appendChild(app.view);

app.stage.interactive = true;

const container = new PIXI.Container();
app.stage.addChild(container);

const flag = PIXI.Sprite.from('https://pixijs.io/pixi-filters/tools/screenshots/dist/original.png');
container.addChild(flag);
flag.x = 100;
flag.y = 100;

const displacementSprite = PIXI.Sprite.from('https://pixijs.io/pixi-filters/tools/screenshots/dist/original.png');
// Make sure the sprite is wrapping.
displacementSprite.texture.baseTexture.wrapMode = PIXI.WRAP_MODES.REPEAT;
const displacementFilter = new PIXI.filters.NoiseFilter(displacementSprite);
displacementFilter.padding = 10;

displacementSprite.position = flag.position;

app.stage.addChild(displacementSprite);

flag.filters = [displacementFilter];

displacementFilter.noise = 0.5;
displacementFilter.seed = 1;

app.ticker.add(() => {
    // Offset the sprite position to make vFilterCoord update to larger value. Repeat wrapping makes sure there's still pixels on the coordinates.
    displacementSprite.x++;
    // Reset x to 0 when it's over width to keep values from going to very huge numbers.
    if (displacementSprite.x > displacementSprite.width) { displacementSprite.x = 0; }
});



  },
};
</script>