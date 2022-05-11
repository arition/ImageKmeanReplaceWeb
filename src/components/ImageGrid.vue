<script lang="ts">
import { Vue, Options } from "vue-class-component";
import { Prop } from "vue-property-decorator";
import type { Pixel as PixelType } from "@/models/pixel";
import Pixel from "@/components/Pixel.vue";

@Options({
  components: { Pixel },
})
export default class ImageGrid extends Vue {
  @Prop({ type: Array, default: [] }) readonly pixels!: PixelType[];
  @Prop({ type: Array, default: [] }) readonly colors!: string[];
  @Prop({ type: Number, default: 50 }) readonly width!: number;
  @Prop({ type: Number, default: 50 }) readonly height!: number;
  @Prop({ type: Number, default: 24 }) readonly size!: number;
  @Prop({ type: Boolean, default: true }) readonly showLabel!: boolean;

  get widthWithPx() {
    return `${this.width}px`;
  }
  get heightWithPx() {
    return `${this.height}px`;
  }
  get sizeWithPx() {
    return `${this.size}px`;
  }
  get groupedPixels() {
    const groups = [];
    for (let i = 0; i < this.pixels.length; i += this.width) {
      groups.push(this.pixels.slice(i, i + this.width));
    }
    return groups;
  }

  changePixelDrawedStatus(rowIndex: number, colIndex: number) {
    const index = rowIndex * this.width + colIndex;
    this.$emit("update:pixelsDrawed", index, !this.pixels[index].drawed);
  }
}
</script>

<template>
  <div class="pixel-grid">
    <div
      class="pixel-row"
      v-for="(rowPixels, rowIndex) in groupedPixels"
      :key="rowIndex"
    >
      <Pixel
        class="pixel"
        :style="{ width: sizeWithPx, height: sizeWithPx }"
        v-for="(pixel, index) in rowPixels"
        :key="index"
        :colorLabel="pixel.colorLabel"
        :colors="colors"
        :showLabel="showLabel"
        :drawed="pixel.drawed"
        @click="() => changePixelDrawedStatus(rowIndex, index)"
      />
    </div>
  </div>
</template>

<style scoped lang="scss">
.pixel-grid {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
}

.pixel-row {
  display: flex;
  &:nth-child(4n + 1) {
    border-bottom: 1px solid #ccc;
  }
}

.pixel {
  flex-shrink: 0;
  flex-grow: 0;

  &:nth-child(4n + 1) {
    border-right: 1px solid #ccc;
  }
}
</style>
