<script lang="ts">
import { Vue, Options } from "vue-class-component";
import { Watch } from "vue-property-decorator";
import type { Pixel } from "@/models/pixel";
import FileInput from "@/components/FileInput.vue";
import ImageGrid from "@/components/ImageGrid.vue";
import ColorInfoPanel from "./components/ColorInfoPanel.vue";

@Options({
  components: {
    FileInput,
    ImageGrid,
    ColorInfoPanel,
  },
})
export default class App extends Vue {
  pixels: Pixel[] = [];
  colors: string[] = [];
  size = 28;
  width = 48;
  height = 64;
  showLabel = true;

  beforeMount() {
    this.pixels = JSON.parse(localStorage.getItem("pixels") ?? "[]") ?? [];
    this.colors = JSON.parse(localStorage.getItem("colors") ?? "[]") ?? [];
    this.size = JSON.parse(localStorage.getItem("size") ?? "28") ?? 28;
  }

  openConfigHandler(config: { labels: number[]; centroids: string[] }) {
    console.log("aaas");
    this.colors = config.centroids;
    this.pixels = config.labels.map((label) => ({
      colorLabel: label,
      drawed: false,
    }));
  }

  updatePixelsDrawedHandler(index: number, drawed: boolean) {
    this.pixels[index].drawed = drawed;
    this.savePixels();
  }

  @Watch("pixels")
  savePixels() {
    localStorage.setItem("pixels", JSON.stringify(this.pixels));
  }
  @Watch("colors")
  saveColors() {
    localStorage.setItem("colors", JSON.stringify(this.colors));
  }
  @Watch("size")
  saveSize() {
    localStorage.setItem("size", JSON.stringify(this.size));
  }

  // get unique labels and its count
  get colorsLabelCount() {
    const labels = this.pixels.map((pixel) => pixel.colorLabel);
    const uniqueLabels = [...new Set(labels)];
    const labelCount = uniqueLabels.reduce(
      (acc, label) => ({
        ...acc,
        [this.colors[label]]: labels.filter((l) => l === label).length,
      }),
      {}
    );
    return labelCount;
  }
}
</script>

<template>
  <h1>Minecraft Pixel Art Building Helper</h1>
  <div class="config">
    <div class="load-config-button">
      <FileInput @openConfig="openConfigHandler" />
    </div>
    <div class="size-input">
      <label>Size:</label>
      <input type="number" v-model="size" />
    </div>
    <div class="size-input">
      <label>Width:</label>
      <input type="number" v-model="width" />
    </div>
    <div class="size-input">
      <label>Height:</label>
      <input type="number" v-model="height" />
    </div>
    <div class="show-label">
      <label>Show label:</label>
      <input type="checkbox" v-model="showLabel" />
    </div>
  </div>
  <ImageGrid
    :pixels="pixels"
    :colors="colors"
    :width="width"
    :height="height"
    :size="size"
    :showLabel="showLabel"
    @update:pixelsDrawed="updatePixelsDrawedHandler"
  />
  <ColorInfoPanel :colors="colors" :colorsLabelCount="colorsLabelCount" />
</template>

<style lang="scss" scoped></style>
<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Inter&display=block");
* {
  box-sizing: border-box;
  font-family: "Inter", sans-serif;
}
.config {
  display: flex;
  flex-direction: column;
  gap: 8px;
}
</style>
