<template>
  <!-- TODO make it so you can move already placed piece
       TODO Add some kind of turn system
       TODO Add a way to win and lose
       TODO Code clean up and refactor
  -->
  <div>
    <transition-group name="list" tag="div" class="grid grid-cols-3 gap-3">
      <drag
        v-for="piece in player1Pieces"
        :key="piece.id"
        class="drag"
        :data="piece"
        @cut="removep1(piece)"
      >
        <gamePiece :pieceColor="piece.pieceColor" :size="piece.size">
        </gamePiece>
      </drag>
    </transition-group>

    <transition-group name="list" tag="div" class="grid grid-cols-3 gap-3">
      <drag
        v-for="piece in player2Pieces"
        :key="piece.id"
        class="drag"
        :data="piece"
        @cut="removep2(piece)"
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
        :accepts-data="
          (p) => {
            if (box1Data == undefined) {
              return true;
            } else if (p.size > box1Data.size) {
              return true;
            }
            return false;
          }
        "
      >
        <span v-if="box1Data">
          <gamePiece :pieceColor="box1Data.pieceColor" :size="box1Data.size">
          </gamePiece>
        </span>
      </drop>

      <drop
        class="border border-white p-10"
        @drop="onCutDrop2($event)"
        mode="cut"
        :accepts-data="
          (p) => {
            if (box2Data == undefined) {
              return true;
            } else if (p.size > box2Data.size) {
              return true;
            }
            return false;
          }
        "
      >
        <span v-if="box2Data">
          <gamePiece :pieceColor="box2Data.pieceColor" :size="box2Data.size">
          </gamePiece
        ></span>
      </drop>

      <drop
        class="border border-white p-10"
        @drop="onCutDrop3($event)"
        mode="cut"
        :accepts-data="
          (p) => {
            if (box3Data == undefined) {
              return true;
            } else if (p.size > box3Data.size) {
              return true;
            }
            return false;
          }
        "
      >
        <span v-if="box3Data">
          <gamePiece :pieceColor="box3Data.pieceColor" :size="box3Data.size">
          </gamePiece
        ></span>
      </drop>

      <drop
        class="border border-white p-10"
        @drop="onCutDrop4($event)"
        mode="cut"
        :accepts-data="
          (p) => {
            if (box4Data == undefined) {
              return true;
            } else if (p.size > box4Data.size) {
              return true;
            }
            return false;
          }
        "
      >
        <span v-if="box4Data">
          <gamePiece :pieceColor="box4Data.pieceColor" :size="box4Data.size">
          </gamePiece
        ></span>
      </drop>

      <drop
        class="border border-white p-10"
        @drop="onCutDrop5($event)"
        mode="cut"
        :accepts-data="
          (p) => {
            if (box5Data == undefined) {
              return true;
            } else if (p.size > box5Data.size) {
              return true;
            }
            return false;
          }
        "
      >
        <span v-if="box5Data">
          <gamePiece :pieceColor="box5Data.pieceColor" :size="box5Data.size">
          </gamePiece
        ></span>
      </drop>

      <drop
        class="border border-white p-10"
        @drop="onCutDrop6($event)"
        mode="cut"
        :accepts-data="
          (p) => {
            if (box6Data == undefined) {
              return true;
            } else if (p.size > box6Data.size) {
              return true;
            }
            return false;
          }
        "
      >
        <span v-if="box6Data">
          <gamePiece :pieceColor="box6Data.pieceColor" :size="box6Data.size">
          </gamePiece
        ></span>
      </drop>

      <drop
        class="border border-white p-10"
        @drop="onCutDrop7($event)"
        mode="cut"
        :accepts-data="
          (p) => {
            if (box7Data == undefined) {
              return true;
            } else if (p.size > box7Data.size) {
              return true;
            }
            return false;
          }
        "
      >
        <span v-if="box7Data">
          <gamePiece :pieceColor="box7Data.pieceColor" :size="box7Data.size">
          </gamePiece
        ></span>
      </drop>

      <drop
        class="border border-white p-10"
        @drop="onCutDrop8($event)"
        mode="cut"
        :accepts-data="
          (p) => {
            if (box8Data == undefined) {
              return true;
            } else if (p.size > box8Data.size) {
              return true;
            }
            return false;
          }
        "
      >
        <span v-if="box8Data">
          <gamePiece :pieceColor="box8Data.pieceColor" :size="box8Data.size">
          </gamePiece
        ></span>
      </drop>

      <drop
        class="border border-white p-10"
        @drop="onCutDrop9($event)"
        mode="cut"
        :accepts-data="
          (p) => {
            if (box9Data == undefined) {
              return true;
            } else if (p.size > box9Data.size) {
              return true;
            }
            return false;
          }
        "
      >
        <span v-if="box9Data">
          <gamePiece :pieceColor="box9Data.pieceColor" :size="box9Data.size">
          </gamePiece
        ></span>
      </drop>
    </div>
  </div>
</template>

<script setup lang="ts">
import GameBoardBox from "./game-board-box.vue";
import gamePiece from "./game-piece.vue";
import { ref, computed, toRaw } from "vue";
import { Drag, Drop, DropMask } from "vue-easy-dnd";

class GamePieceData {
  constructor(
    public id: number,
    public pieceColor: string,
    public size: number
  ) {
    this.id = id;
    this.pieceColor = pieceColor;
    this.size = size;
  }
}

const allPieces = ref([
  new GamePieceData(1, "red", 1),
  new GamePieceData(2, "blue", 1),
  new GamePieceData(3, "red", 1),
]);

const player1Pieces = ref([
  new GamePieceData(1, "red", 1),
  new GamePieceData(2, "red", 1),
  new GamePieceData(3, "red", 2),
  new GamePieceData(4, "red", 2),
  new GamePieceData(5, "red", 3),
  new GamePieceData(6, "red", 3),
]);

const player2Pieces = ref([
  new GamePieceData(1, "blue", 1),
  new GamePieceData(2, "blue", 1),
  new GamePieceData(3, "blue", 2),
  new GamePieceData(4, "blue", 2),
  new GamePieceData(5, "blue", 3),
  new GamePieceData(6, "blue", 3),
]);

const box1Data = ref();
const box2Data = ref();
const box3Data = ref();
const box4Data = ref();
const box5Data = ref();
const box6Data = ref();
const box7Data = ref();
const box8Data = ref();
const box9Data = ref();

//this is terrible
function onCutDrop1(e: Event) {
  box1Data.value = e.data;
}
function onCutDrop2(e: Event) {
  box2Data.value = e.data;
}
function onCutDrop3(e: Event) {
  box3Data.value = e.data;
}

function onCutDrop4(e: Event) {
  box4Data.value = e.data;
}
function onCutDrop5(e: Event) {
  box5Data.value = e.data;
}
function onCutDrop6(e: Event) {
  box6Data.value = e.data;
}

function onCutDrop7(e: Event) {
  box7Data.value = e.data;
}
function onCutDrop8(e: Event) {
  box8Data.value = e.data;
}
function onCutDrop9(e: Event) {
  box9Data.value = e.data;
}

function checkCanPlace(piece1: GamePieceData, piece2: GamePieceData) {
  console.log(toRaw(piece1));
  if (piece1 === undefined || toRaw(piece2).size > piece1.size) {
    return true;
  }
  return false;
}

//just going to comment out the remove logic for now because it'll remove stuff even if you aren't allowed to place a piece
function removep1(n) {
  // let index = player1Pieces.value.indexOf(n);
  // player1Pieces.value.splice(index, 1);
}

function removep2(n) {
  // let index = player2Pieces.value.indexOf(n);
  // player2Pieces.value.splice(index, 1);
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
