<template>
  <div>
    <div v-if="size === 1">
      <font-awesome-icon
        icon="fa-solid fa-dice-one"
        :color="pieceColor"
        :size="iconSize"
      />
    </div>

    <div v-else-if="size === 2">
      <font-awesome-icon
        icon="fa-solid fa-dice-two"
        :color="pieceColor"
        :size="iconSize"
      />
    </div>

    <div v-else-if="size === 3">
      <font-awesome-icon
        icon="fa-solid fa-dice-three"
        :color="pieceColor"
        :size="iconSize"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

const props = defineProps({
  pieceColor: { type: String, default: "red", required: true },
  size: { type: Number, default: 1, required: true },
  iconSize: { type: String, default: "6x", required: true },
});
</script>
