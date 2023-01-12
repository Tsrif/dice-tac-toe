<!-- used this as a base https://codesandbox.io/s/multidraganddropmultiplelists-w2426?file=/src/App.vue -->
<template>
  <div>{{ currentTurn }}</div>
  <transition-group name="list" tag="div" class="grid grid-cols-3 gap-3">
    <drag
      v-for="piece in player1Pieces"
      :key="piece.id"
      :disabled="currentTurn == gameStates.player1Turn ? false : true"
      class="drag"
      :data="piece"
      @cut="remove(player1Pieces, piece)"
    >
      <gamePiece :pieceColor="piece.pieceColor" :size="piece.size"> </gamePiece>
    </drag>
  </transition-group>

  <transition-group name="list" tag="div" class="grid grid-cols-3 gap-3">
    <drag
      v-for="piece in player2Pieces"
      :key="piece.id"
      :disabled="currentTurn == gameStates.player2Turn ? false : true"
      class="drag"
      :data="piece"
      @cut="remove(player2Pieces, piece)"
    >
      <gamePiece :pieceColor="piece.pieceColor" :size="piece.size"> </gamePiece>
    </drag>
  </transition-group>

  <div fluid class="wrapper grid grid-cols-3 gap-3">
    <!--START DRAG DROP BOX -->
    <div v-for="(box, index) in boxes" :key="index">
      <drop-list
        :items="box.value"
        class="list"
        @insert="onInsert($event, 'box' + (index + 1) + 'Data')"
        @reorder="$event.apply(box.value)"
        mode="cut"
        :accepts-data="(p) => checkAccept(p, box)"
      >
        <!-- gamepiece / inside the droplist -->
        <template v-slot:item="{ item }">
          <div>
            <drag
              v-if="item === box.value[box.value.length - 1]"
              class="item"
              :class="{ selected: selected.indexOf(item) > -1 }"
              :disabled="setDisabled(item.pieceColor)"
              @click="toggleSelected(box.value, item)"
              @cut="remove(box.value, item)"
              :data="selection(item)"
              :key="item.id"
            >
              <gamePiece :pieceColor="item.pieceColor" :size="item.size">
              </gamePiece>
            </drag>
            <gamePiece
              v-else
              :pieceColor="item.pieceColor"
              :size="item.size"
              icon-size="2x"
            >
            </gamePiece>
          </div>
        </template>

        <!-- You have to have this feedback slot thing. idk why -->
        <template v-slot:feedback="{ data }">
          <template v-if="selected.length > 0">
            <div></div>
          </template>
          <template v-else>
            <div></div>
          </template>
        </template>
        <!-- end feedback slot -->
      </drop-list>
    </div>
    <!--END DRAG DROP BOX -->
  </div>
</template>
  
  <script setup lang="ts">
import { Drag, DropList } from "vue-easy-dnd";
import { ref } from "vue";
import gamePiece from "./game-piece.vue";

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

const player1Pieces = ref([
  new GamePieceData(1, "red", 1),
  new GamePieceData(2, "red", 1),
  new GamePieceData(3, "red", 2),
  new GamePieceData(4, "red", 2),
  new GamePieceData(5, "red", 3),
  new GamePieceData(6, "red", 3),
]);

const player2Pieces = ref([
  new GamePieceData(7, "blue", 1),
  new GamePieceData(8, "blue", 1),
  new GamePieceData(9, "blue", 2),
  new GamePieceData(10, "blue", 2),
  new GamePieceData(11, "blue", 3),
  new GamePieceData(12, "blue", 3),
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

const boxes = ref([
  box1Data,
  box2Data,
  box3Data,
  box4Data,
  box5Data,
  box6Data,
  box7Data,
  box8Data,
  box9Data,
]);

const selected = ref([]);
const selectedList = ref(0);

const gameStates = {
  player1Turn: "Player 1",
  player2Turn: "Player 2",
  gameOver: "Game Over",
};

const currentTurn = ref(gameStates.player1Turn);

function selection(item) {
  return this.selected.length > 0 ? this.selected : item;
}
/**
 * Inserts one or multiple selected items into the target
 * droplist
 *
 * @event {InsertEvent} - holds dragging data and drop index
 * @llistNameist String - name of the list in the data section
 */
function onInsert(event, listName = "items") {
  this[listName].push(event.data);
  checkWinner();

  //tbh I don't know if this is needed
  selected.value = [];
}
function remove(array, value) {
  // Following logic is taken from https://codesandbox.io/s/easy-dnd-demo-9mbij
  // In addition if we have some items in the selection
  // we apply the same logic just with a loop

  if (selected.value.length > 0) {
    selected.value.forEach((e) => {
      let index = array.indexOf(e);
      array.splice(index, 1);
    });
  } else {
    let index = array.indexOf(value);
    array.splice(index, 1);
  }
}
/**
 * Inserts an item into the selected list.
 *
 * If you select an item from another list the selection of
 * the last list gets resetted.
 *
 * @see https://codesandbox.io/s/easy-dnd-demo-2-xnqbz
 * @listName String - name of the list in the data section
 * @item {Object} - the selected data in the list
 */
function toggleSelected(listName, item) {
  if (listName !== selectedList.value) {
    selected.value = [];
    selectedList.value = listName;
  }

  // Basic toggeling logic
  // If an item is in the list remove it
  // otherwise add it to the list
  const index = selected.value.indexOf(item);
  if (index > -1) {
    selected.value.splice(index, 1);
  } else {
    selected.value.push(item);
  }
}

function setDisabled(color) {
  if (color == "red" && currentTurn.value == gameStates.player1Turn) {
    return false;
  } else if (color == "blue" && currentTurn.value == gameStates.player2Turn) {
    return false;
  } else return true;
}

function checkAccept(p, box) {
  if (box.value == undefined || box.value.length === 0) {
    return true;
  } else if (p.size > box.value[box.value.length - 1].size) {
    return true;
  }
  return false;
}

function checkWinner() {
  var playerColor;

  if (currentTurn.value == gameStates.player1Turn) {
    playerColor = "red";
  } else {
    playerColor = "blue";
  }

  //might be ineffiencent to create this everytime?
  var board = [
    [box1Data, box2Data, box3Data],
    [box4Data, box5Data, box6Data],
    [box7Data, box8Data, box9Data],
  ];

  //Horizontal
  for (let i = 0; i < 3; i++) {
    if (
      board[i][0].value.at(-1)?.pieceColor === playerColor &&
      board[i][1].value.at(-1)?.pieceColor === playerColor &&
      board[i][2].value.at(-1)?.pieceColor === playerColor
    ) {
      console.log("HORIZONTAL WINNER");
      currentTurn.value = gameStates.gameOver;
      return true;
    }
  }

  //Vertical
  for (let i = 0; i < 3; i++) {
    if (
      board[0][i].value.at(-1)?.pieceColor === playerColor &&
      board[1][i].value.at(-1)?.pieceColor === playerColor &&
      board[2][i].value.at(-1)?.pieceColor === playerColor
    ) {
      console.log("VERTICAL WINNER");
      currentTurn.value = gameStates.gameOver;
      return true;
    }
  }

  //Diagonals
  if (
    board[0][0].value.at(-1)?.pieceColor === playerColor &&
    board[1][1].value.at(-1)?.pieceColor === playerColor &&
    board[2][2].value.at(-1)?.pieceColor === playerColor
  ) {
    console.log("DIAGONAL WINNER");
    currentTurn.value = gameStates.gameOver;
    return true;
  }
  if (
    board[1][0].value.at(-1)?.pieceColor === playerColor &&
    board[1][1].value.at(-1)?.pieceColor === playerColor &&
    board[0][2].value.at(-1)?.pieceColor === playerColor
  ) {
    console.log("DIAGONAL WINNER");
    currentTurn.value = gameStates.gameOver;
    return true;
  }

  console.log("no winner found, swapping turns");
  //swap current turn
  currentTurn.value =
    currentTurn.value == gameStates.player1Turn
      ? gameStates.player2Turn
      : gameStates.player1Turn;
  return false;
}
</script>
  
  <style>
.drag {
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

.wrapper .list {
  border: 1px solid white;
  width: 200px;
  min-height: 200px;
}
.wrapper .list .item {
  /* padding: 20px;
  margin: 10px;
  background-color: #131342; */
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
}
/* .wrapper .list .item.selected {
  border: 2px solid red;
} */
/* .wrapper .list .item.feedback {
  background-color: #ffdcdc;
  border: 2px dashed black;
} */
.wrapper .list .item.drag-image {
  background-color: #dcffdc;
  transform: translate(-50%, -50%);
}
</style>