<script lang="ts">
import { Options, Vue } from "vue-class-component";
import { Prop } from "vue-property-decorator";
import ColorInfo from "./ColorInfo.vue";

@Options({
  components: { ColorInfo },
})
export default class ColorInfoPanel extends Vue {
  @Prop({ type: Array, default: [] }) readonly colors!: string[];
  @Prop({ type: Object, default: {} }) readonly colorsLabelCount!: Record<
    string,
    number
  >;
}
</script>

<template>
  <div class="color-info-panel">
    <ColorInfo
      v-for="(color, index) in colors"
      :key="index"
      :color="color"
      :colorLabelCount="colorsLabelCount[color]"
    />
  </div>
</template>

<style scoped lang="scss">
.color-info-panel {
  position: fixed;
  top: 50%;
  right: 0;
  transform: translateY(-50%);

  display: flex;
  flex-direction: column;
  gap: 4px;

  padding: 8px;
  border-radius: 4px;
  background-color: rgba(255, 255, 255, 0.8);
}
</style>
