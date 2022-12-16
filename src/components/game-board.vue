<!-- <template>
  <div>
    <gamePiece
      pieceColor="red"
      size="2"
      id="1"
      @sendData="captureGamePieceData"
    ></gamePiece>
    <gamePiece pieceColor="red" size="2"></gamePiece>
    <gamePiece pieceColor="red" size="2"></gamePiece>
    <gamePiece pieceColor="blue" size="2"></gamePiece>
    <gamePiece pieceColor="blue" size="2"></gamePiece>
    <gamePiece pieceColor="blue" size="2"></gamePiece>
    <div class="grid grid-cols-3 gap-3">
      <div>
        <GameBoardBox>
          <gamePiece pieceColor="blue" size="2"></gamePiece>
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
</template> -->

<template>
  <div id="app">
    <transition-group name="list" tag="div">
      <drag
        v-for="piece in allPieces"
        :key="piece.id"
        class="drag"
        :data="piece"
        @cut="remove(piece)"
      >
        <gamePiece :pieceColor="piece.pieceColor" :size="piece.size">
        </gamePiece>
      </drag>
    </transition-group>
    <div class="group">
      <drop class="copy" @drop="onCopyDrop">
        <span v-for="(n, index) in copied" :key="index"
          >Copied {{ n }};&nbsp;</span
        >
      </drop>
      <drop class="cut" @drop="onCutDrop" mode="cut">
        <span v-for="(piece, index) in cut" :key="index"
          >Cut
          <gamePiece :pieceColor="piece.pieceColor" :size="piece.size">
          </gamePiece
          >;&nbsp;</span
        >
      </drop>
    </div>
  </div>
</template>

<script setup lang="ts">
import GameBoardBox from "./game-board-box.vue";
import gamePiece from "./game-piece.vue";
import { ref, computed } from "vue";
import { Drag, Drop, DropMask } from "vue-easy-dnd";

class GamePieceData {
  constructor(
    public id: number,
    public position: string,
    public pieceColor: string,
    public size: string
  ) {
    this.id = id;
    this.position = position;
    this.pieceColor = pieceColor;
    this.size = size;
  }
}

const allPieces = ref([
  new GamePieceData(1, "0", "red", "1"),
  new GamePieceData(2, "0", "red", "1"),
  new GamePieceData(3, "0", "red", "1"),
]);
const copied = ref([]);
const cut = ref([]);

function onCopyDrop(e) {
  copied.value.push(e.data);
}
function onCutDrop(e) {
  cut.value.push(e.data);
}
function remove(n) {
  let index = allPieces.value.indexOf(n);
  allPieces.value.splice(index, 1);
}
</script>

<style>
html,
body {
  height: 100%;
  font-family: "Roboto";
}

.drag {
  width: 60px;
  height: 60px;
  background-color: rgb(220, 220, 255);
  display: inline-flex;
  align-items: center;
  justify-content: center;
  margin: 10px 10px 0 10px;
  font-size: 20px;
  transition: all 0.5s;
}

.group {
  display: flex;
}

.copy {
  margin: 20px 10px;
  border: 1px solid black;
  height: 100px;
  display: inline-block;
  position: relative;
  flex: 1;
}

.cut {
  margin: 20px 10px;
  border: 1px solid black;
  height: 100px;
  display: inline-block;
  position: relative;
  flex: 1;
}

.copy::before {
  content: "COPY";
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  color: rgba(0, 0, 0, 0.4);
  font-size: 25px;
  font-weight: bold;
}

.cut::before {
  content: "CUT";
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  color: rgba(0, 0, 0, 0.4);
  font-size: 25px;
  font-weight: bold;
}

.drop-allowed {
  background-color: rgba(0, 255, 0, 0.2);
}

.drop-forbidden {
  background-color: rgba(255, 0, 0, 0.2);
}

.drop-in {
  box-shadow: 0 0 5px rgba(0, 0, 255, 0.4);
}

.list-enter,
.list-leave-to {
  opacity: 0;
}

.list-leave-active {
  position: absolute;
}
</style>
