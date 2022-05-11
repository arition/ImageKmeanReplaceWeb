<script lang="ts">
import { Vue } from "vue-class-component";
import { Prop } from "vue-property-decorator";

export default class Pixel extends Vue {
  @Prop({ type: Number }) readonly colorLabel!: number;
  @Prop({ type: Array, default: [] }) readonly colors!: string[];
  @Prop({ type: Boolean, default: true }) readonly showLabel!: boolean;
  @Prop({ type: Boolean, default: false }) readonly drawed!: boolean;

  get color() {
    return this.colors[this.colorLabel];
  }
}
</script>

<template>
  <div
    :class="{ pixel: true, drawed: drawed }"
    :style="{ backgroundColor: color }"
  >
    <div :class="{ label: true, show: showLabel }">{{ colorLabel }}</div>
    <div :class="{ cross: true, show: drawed }">
      <svg viewBox="0 0 24 24">
        <path
          d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12z"
        />
      </svg>
    </div>
  </div>
</template>

<style scoped lang="scss">
.pixel {
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;

  cursor: pointer;
  transition: all 0.1s ease-in;

  &:hover {
    opacity: 0.7;
    transition: all 0.1s ease-out;
  }

  &.drawed .label {
    display: none;
  }

  .cross {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: none;

    background-color: rgba(255, 255, 255, 0.5);
    fill: rgb(0, 0, 0);
    &.show {
      display: block;
    }
  }
}

.label {
  display: none;
  color: white;
  mix-blend-mode: difference;
  opacity: 0.4;
  &.show {
    display: block;
  }
}
</style>
