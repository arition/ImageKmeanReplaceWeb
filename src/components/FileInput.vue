<template>
  <button @click="readConfig">Choose json config</button>
</template>

<script lang="ts">
import { Vue } from "vue-class-component";

export default class FileInput extends Vue {
  async readConfig() {
    const [fileHandle] = await window.showOpenFilePicker();
    const file = await fileHandle.getFile();
    const content = await file.text();
    const { labels, centroids } = JSON.parse(content) as {
      labels: number[];
      centroids: string[];
    };
    this.$emit("openConfig", { labels, centroids });
  }
}
</script>
