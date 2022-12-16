<template>
  <div
    class="p-1"
    draggable="true"
    @dragstart="startDrag($event, id, position, pieceColor, size)"
  >
    <div class="inline-block" :style="{ 'background-color': pieceColor }">
      {{ pieceColor }} : {{ size }}
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

const props = defineProps({
  id: { type: String, default: "0", required: true },
  position: { type: String, default: "0", required: true },
  pieceColor: { type: String, default: "red", required: true },
  size: { type: String, default: "1", required: true },
});

const emit = defineEmits(["sendData"]);

function startDrag(evt, id, position, pieceColor, size) {
  evt.dataTransfer.dropEffect = "move";
  evt.dataTransfer.effectAllowed = "move";
  // evt.dataTransfer.setData("itemID", item.id);
  emit("sendData", "aye yo from " + props.id);
  console.log("----START DRAG----");
  console.log(id);
  console.log(position);
  console.log(pieceColor);
  console.log(size);
}

function onDrop(evt, id, position, pieceColor, size) {
  // const itemID = evt.dataTransfer.getData("itemID");
  // const item = this.items.find((item) => item.id == itemID);
  // item.list = list;
  console.log("----ON DROP----");
  console.log(id);
  console.log(position);
  console.log(pieceColor);
  console.log(size);
}

// export default {
//   methods: {
//     startDrag(evt, id) {
//       evt.dataTransfer.dropEffect = "move";
//       evt.dataTransfer.effectAllowed = "move";
//       // evt.dataTransfer.setData("itemID", item.id);
//       console.log(id);
//     },
//     onDrop(evt, list) {
//       const itemID = evt.dataTransfer.getData("itemID");
//       const item = this.items.find((item) => item.id == itemID);
//       item.list = list;
//     },
//   },
//   props: {
//     id: { type: String, default: "0", required: true },
//     position: { type: String, default: "0", required: true },
//     pieceColor: { type: String, default: "red", required: true },
//     size: { type: String, default: "1", required: true },
//   },
// };
</script>
