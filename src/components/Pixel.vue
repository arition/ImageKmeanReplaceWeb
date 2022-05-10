<script lang="ts">
import { Vue } from "vue-class-component";
import { Prop } from "vue-property-decorator";

export default class Pixel extends Vue {
  @Prop({ type: Number }) readonly colorLabel!: number;
  @Prop({ type: Array, default: [] }) readonly colors!: string[];
  // @Model("drawed", { type: Boolean, default: false }) readonly drawed!: boolean;
  drawed = false;

  get color() {
    return this.colors[this.colorLabel];
  }
}
</script>

<template>
  <div
    @click="() => (drawed = !drawed)"
    :class="{ pixel: true, drawed: drawed }"
    :style="{ backgroundColor: !drawed ? color : '#555' }"
  >
    <div class="label">{{ colorLabel }}</div>
  </div>
</template>

<style scoped lang="scss">
.pixel {
  display: flex;
  align-items: center;
  justify-content: center;

  cursor: pointer;
  transition: all 0.2s ease-in-out;

  &:hover {
    opacity: 0.7;
  }

  &.drawed .label {
    display: none;
  }
}

.label {
  color: white;
  mix-blend-mode: difference;
}
</style>
