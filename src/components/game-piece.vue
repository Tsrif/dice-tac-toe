<template>
  <div class="p-3">
    <div
      v-if="size === 1"
      class="h-5 w-5 rotate-45 transform origin-bottom-left"
      :style="{ 'background-color': pieceColor }"
    >
      {{ size }}
    </div>

    <div
      v-else-if="size === 2"
      class="p-2 inline-block"
      :style="{ 'background-color': pieceColor }"
    >
      {{ size }}
    </div>

    <div
      v-else-if="size === 3"
      class="p-2 rounded-full inline-block items-center justify-center"
      :style="{ 'background-color': pieceColor }"
    >
      {{ size }}
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

const props = defineProps({
  pieceColor: { type: String, default: "red", required: true },
  size: { type: Number, default: 1, required: true },
});
</script>
