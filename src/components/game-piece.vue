<template>
  <div class="p-1">
    <div class="inline-block" :style="{ 'background-color': pieceColor }">
      {{ pieceColor }} : {{ size }}
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

const props = defineProps({
  id: { type: String, default: "0", required: true },
  pieceColor: { type: String, default: "red", required: true },
  size: { type: String, default: "1", required: true },
});
</script>
