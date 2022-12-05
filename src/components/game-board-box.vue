<template>
  <div
    class="border border-white w-auto p-10 drop-zone"
    @drop="onDrop($event, 1)"
    @dragover.prevent
    @dragenter.prevent
  >
    <!-- where the game piece goes 
      I'm thinking I could always have a game piece in here, and then when you drop a new game piece in here, it'll just take all the data from the new one and apply it to the one that's in here already???
    -->
    <slot></slot>
  </div>
</template>

<script lang="ts">
export default {
  methods: {
    onDrop(evt, list) {
      console.log(evt);
      // const item = this.items.find((item) => item.id == itemID);
      // item.list = list;
    },
  },
};
</script>
