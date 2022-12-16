<template>
  <div
    class="border border-white w-auto p-10 drop-zone"
    @drop="onDrop($event)"
    @dragover.prevent
    @dragenter.prevent
  >
    <!-- where the game piece goes 
      I'm thinking I could always have a game piece in here, and then when you drop a new game piece in here, it'll just take all the data from the new one and apply it to the one that's in here already???
    -->
    <!-- <slot></slot> -->
    <!-- <gamePiece pieceColor="blue" size="2"></gamePiece> -->
  </div>
</template>

<script setup lang="ts">
import { computed } from "vue";
import gamePiece from "./game-piece.vue";

function onDrop(evt) {
  console.log("----ON DROP----");
  console.log(evt);
  //console.log(getDataFromSlot);
  // const item = this.items.find((item) => item.id == itemID);
  // item.list = list;
}

const getDataFromSlot = computed(() => {
  return this.$slot.data;
});

// export default {
//   methods: {
//     onDrop(evt) {
//       console.log("----ON DROP----");
//       console.log(evt);
//       // const item = this.items.find((item) => item.id == itemID);
//       // item.list = list;
//     },
//   },
// };
</script>
