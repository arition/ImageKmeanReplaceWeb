<script lang="ts">
import { Vue, Options } from "vue-class-component";
import type { Pixel } from "@/models/pixel";
import FileInput from "@/components/fileInput.vue";
import ImageGrid from "@/components/imageGrid.vue";

@Options({
  components: {
    FileInput,
    ImageGrid,
  },
})
export default class App extends Vue {
  pixels: Pixel[] = [];
  colors: string[] = [];

  openConfigHandler(config: { labels: number[]; centroids: string[] }) {
    this.colors = config.centroids;
    this.pixels = config.labels.map((label) => ({
      colorLabel: label,
      drawed: false,
    }));
  }
}
</script>

<template>
  <h1>Minecraft Pixel Art Building Helper</h1>
  <FileInput @openConfig="openConfigHandler" />
  <ImageGrid :pixels="pixels" :colors="colors" :width="50" :height="50" />
</template>

<style lang="scss" scoped></style>
<style lang="scss">
* {
  box-sizing: border-box;
}
</style>
