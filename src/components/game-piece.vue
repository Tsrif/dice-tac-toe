<template>
  <div class="p-1" draggable="true" @dragstart="startDrag($event, id)">
    <div class="inline-block" :style="{ 'background-color': pieceColor }">
      {{ pieceColor }} : {{ size }}
    </div>
  </div>
</template>

<script lang="ts">
export default {
  methods: {
    startDrag(evt, id) {
      evt.dataTransfer.dropEffect = "move";
      evt.dataTransfer.effectAllowed = "move";
      // evt.dataTransfer.setData("itemID", item.id);
      console.log(id);
    },
    onDrop(evt, list) {
      const itemID = evt.dataTransfer.getData("itemID");
      const item = this.items.find((item) => item.id == itemID);
      item.list = list;
    },
  },
  props: {
    id: { type: String, default: "0", required: true },
    position: { type: String, default: "0", required: true },
    pieceColor: { type: String, default: "red", required: true },
    size: { type: String, default: "1", required: true },
  },
};
</script>
