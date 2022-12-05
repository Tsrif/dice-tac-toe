<template>
  <div>
    <gamePiece pieceColor="red" size="2" id="1"></gamePiece>
    <gamePiece pieceColor="red" size="2"></gamePiece>
    <gamePiece pieceColor="red" size="2"></gamePiece>
    <gamePiece pieceColor="blue" size="2"></gamePiece>
    <gamePiece pieceColor="blue" size="2"></gamePiece>
    <gamePiece pieceColor="blue" size="2"></gamePiece>
    <div class="grid grid-cols-3 gap-3">
      <div>
        <GameBoardBox>
          <!-- <gamePiece pieceColor="blue" size="2"></gamePiece> -->
        </GameBoardBox>
      </div>
      <div><GameBoardBox></GameBoardBox></div>
      <div><GameBoardBox></GameBoardBox></div>

      <div><GameBoardBox></GameBoardBox></div>
      <div><GameBoardBox></GameBoardBox></div>
      <div><GameBoardBox></GameBoardBox></div>

      <div><GameBoardBox></GameBoardBox></div>
      <div><GameBoardBox></GameBoardBox></div>
      <div><GameBoardBox></GameBoardBox></div>
    </div>
  </div>
</template>

<script lang="ts">
import GameBoardBox from "./game-board-box.vue";
import gamePiece from "./game-piece.vue";
export default {
  components: { GameBoardBox, gamePiece },
  data() {
    return {};
  },
};
</script>
