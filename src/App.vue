<template>
  <div>
    <GameBoard />
  </div>
</template>

<script lang="ts">
import GameBoard from "./components/game-board.vue";
export default {
  name: "App",
  components: { GameBoard },
};
</script>
