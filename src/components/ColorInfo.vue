<script lang="ts">
import { Vue } from "vue-class-component";
import { Prop, Watch } from "vue-property-decorator";

export default class ColorInfo extends Vue {
  @Prop({ type: String, default: "" }) readonly color!: string;
  @Prop({ type: Number, default: 0 }) readonly colorLabelCount!: number;
  comment = "";

  beforeMount() {
    this.comment = localStorage.getItem(this.color) ?? "";
  }

  @Watch("comment")
  saveComment() {
    localStorage.setItem(this.color, this.comment);
  }
}
</script>

<template>
  <div class="color-info">
    <div class="color-patch" :style="{ backgroundColor: color }"></div>
    <div class="color-name">{{ color }}</div>
    <div class="color-label-count">{{ colorLabelCount }}</div>
    <input type="text" class="comment" v-model="comment" />
  </div>
</template>

<style scoped lang="scss">
.color-info {
  display: flex;
  gap: 4px;
  align-items: center;
}

.color-patch {
  width: 24px;
  height: 24px;
  border-radius: 50%;
}

.color-name,
.color-label-count {
  font-family: monospace;
}

.color-label-count {
  min-width: 32px;
  text-align: right;
}

.comment {
  width: 64px;
  resize: none;
}
</style>
