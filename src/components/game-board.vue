<template>
  <div>
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
    <div class="grid grid-cols-3 gap-3">
      <drop
        class="border border-white p-10"
        @drop="onCutDrop1($event)"
        mode="cut"
      >
        <span v-for="(piece, index) in box1Data" :key="index">
          <gamePiece :pieceColor="piece.pieceColor" :size="piece.size">
          </gamePiece>
        </span>
      </drop>

      <drop
        class="border border-white p-10"
        @drop="onCutDrop2($event)"
        mode="cut"
      >
        <span v-for="(piece, index) in box2Data" :key="index">
          <gamePiece :pieceColor="piece.pieceColor" :size="piece.size">
          </gamePiece
        ></span>
      </drop>

      <drop
        class="border border-white p-10"
        @drop="onCutDrop3($event)"
        mode="cut"
      >
        <span v-for="(piece, index) in box3Data" :key="index">
          <gamePiece :pieceColor="piece.pieceColor" :size="piece.size">
          </gamePiece
        ></span>
      </drop>

      <drop
        class="border border-white p-10"
        @drop="onCutDrop4($event)"
        mode="cut"
      >
        <span v-for="(piece, index) in box4Data" :key="index">
          <gamePiece :pieceColor="piece.pieceColor" :size="piece.size">
          </gamePiece
        ></span>
      </drop>

      <drop
        class="border border-white p-10"
        @drop="onCutDrop5($event)"
        mode="cut"
      >
        <span v-for="(piece, index) in box5Data" :key="index">
          <gamePiece :pieceColor="piece.pieceColor" :size="piece.size">
          </gamePiece
        ></span>
      </drop>

      <drop
        class="border border-white p-10"
        @drop="onCutDrop6($event)"
        mode="cut"
      >
        <span v-for="(piece, index) in box6Data" :key="index">
          <gamePiece :pieceColor="piece.pieceColor" :size="piece.size">
          </gamePiece
        ></span>
      </drop>

      <drop
        class="border border-white p-10"
        @drop="onCutDrop7($event)"
        mode="cut"
      >
        <span v-for="(piece, index) in box7Data" :key="index">
          <gamePiece :pieceColor="piece.pieceColor" :size="piece.size">
          </gamePiece
        ></span>
      </drop>

      <drop
        class="border border-white p-10"
        @drop="onCutDrop8($event)"
        mode="cut"
      >
        <span v-for="(piece, index) in box8Data" :key="index">
          <gamePiece :pieceColor="piece.pieceColor" :size="piece.size">
          </gamePiece
        ></span>
      </drop>

      <drop
        class="border border-white p-10"
        @drop="onCutDrop9($event)"
        mode="cut"
      >
        <span v-for="(piece, index) in box9Data" :key="index">
          <gamePiece :pieceColor="piece.pieceColor" :size="piece.size">
          </gamePiece
        ></span>
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
    public pieceColor: string,
    public size: string
  ) {
    this.id = id;
    this.pieceColor = pieceColor;
    this.size = size;
  }
}

const allPieces = ref([
  new GamePieceData(1, "red", "1"),
  new GamePieceData(2, "blue", "1"),
  new GamePieceData(3, "red", "1"),
]);

const box1Data = ref([]);
const box2Data = ref([]);
const box3Data = ref([]);
const box4Data = ref([]);
const box5Data = ref([]);
const box6Data = ref([]);
const box7Data = ref([]);
const box8Data = ref([]);
const box9Data = ref([]);

//this is terrible
function onCutDrop1(e: Event) {
  box1Data.value.splice(0, 1, e.data);
  //box1Data.value.cut(e.data);
}
function onCutDrop2(e: Event) {
  box2Data.value.splice(0, 1, e.data);
}
function onCutDrop3(e: Event) {
  box3Data.value.splice(0, 1, e.data);
}

function onCutDrop4(e: Event) {
  box4Data.value.splice(0, 1, e.data);
}
function onCutDrop5(e: Event) {
  box5Data.value.splice(0, 1, e.data);
}
function onCutDrop6(e: Event) {
  box6Data.value.splice(0, 1, e.data);
}

function onCutDrop7(e: Event) {
  box7Data.value.splice(0, 1, e.data);
}
function onCutDrop8(e: Event) {
  box8Data.value.splice(0, 1, e.data);
}
function onCutDrop9(e: Event) {
  box9Data.value.splice(0, 1, e.data);
}

function remove(n) {
  let index = allPieces.value.indexOf(n);
  allPieces.value.splice(index, 1);
}
</script>

<style>
.drag {
  /* width: 60px;
  height: 60px; */
  /* background-color: rgb(220, 220, 255); */
  /* display: inline-flex;
  align-items: center;
  justify-content: center;
  margin: 10px 10px 0 10px;
  font-size: 20px; */
  transition: all 0.5s;
}

.cut {
  margin: 20px 10px;
  border: 1px solid black;
  height: 100px;
  display: inline-block;
  position: relative;
  flex: 1;
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
