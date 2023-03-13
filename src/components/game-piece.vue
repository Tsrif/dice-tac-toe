<template>
  <div>
    <div v-if="size === 1">
      <font-awesome-icon icon="fa-solid fa-dice-one" :color="actualPieceColor" :size="iconSize" />
    </div>

    <div v-else-if="size === 2">
      <font-awesome-icon icon="fa-solid fa-dice-two" :color="actualPieceColor" :size="iconSize" />
    </div>

    <div v-else-if="size === 3">
      <font-awesome-icon icon="fa-solid fa-dice-three" :color="actualPieceColor" :size="iconSize" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from "vue";

const props = defineProps({
  pieceColor: { type: String, default: "red", required: true },
  size: { type: Number, default: 1, required: true },
  iconSize: { type: String, default: "6x", required: true },
});

const actualPieceColor = computed(() => {
  if (props.pieceColor === "red") {
    return "#F43F5E";
  } else if (props.pieceColor === "blue") {
    return "#0EA5E9";
  } else return "bg-gray-500";
});
</script>
