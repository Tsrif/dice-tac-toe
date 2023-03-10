<!-- used this as a base https://codesandbox.io/s/multidraganddropmultiplelists-w2426?file=/src/App.vue -->
<template>
  <div class="justify-center min-w-max">
    <!-- TOP BAR -->
    <div class="flex justify-center text-lg">
      <!-- TURN ORDER -->
      <div class="flex ml-auto" v-if="!winner">
        <div class="w-10 h-10 rounded" :class="
          currentTurn == gameStates.player1Turn ? 'bg-[#F43F5E]' : 'bg-gray-500'
        "></div>

        <div class="px-5">TURN</div>
        <div class="w-10 h-10 rounded" :class="
          currentTurn == gameStates.player2Turn ? 'bg-[#0EA5E9]' : 'bg-gray-500'
        "></div>
      </div>
      <div class="flex ml-auto text-5xl uppercase" v-else>
        <b :class="
          winner == 'red' ? 'text-[#F43F5E]' : 'text-[#0EA5E9]'
        ">{{ winner }}</b> wins

      </div>
      <!-- RULES -->
      <div class="ml-auto">
        <rules></rules>
      </div>
    </div>
    <!-- GAMEBOARD -->
    <div fluid class="wrapper grid grid-cols-3 gap-2 p-5">
      <!--START DRAG DROP BOX -->
      <div v-for="(box, index) in boxes" :key="index">
        <drop-list :items="box.value.pieces" :class="box.value.containsWinningPiece ? 'bg-green-700' : ''" class="list"
          @insert="onInsert($event, index)" @reorder="$event.apply(box.value.pieces)" mode="cut"
          :accepts-data="(p) => checkAccept(p, box)">
          <!-- gamepiece / inside the droplist -->
          <template v-slot:item="{ item }">
            <div>
              <drag v-if="item === box.value.pieces[box.value.pieces.length - 1]" class="item"
                :class="{ selected: selected.indexOf(item as never) > -1 }" :disabled="setDisabled(item.pieceColor)"
                @click="toggleSelected(box.value.pieces, item)" @cut="remove(box.value.pieces, item)"
                :data="selection(item)" :key="item.id">
                <gamePiece :pieceColor="item.pieceColor" :size="item.size">
                </gamePiece>
              </drag>
              <gamePiece v-else :pieceColor="item.pieceColor" :size="item.size" icon-size="2x">
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
    <!-- PLAYER 1 PIECES -->
    <transition-group name="list" tag="div" class="flex justify-center ">
      <drag v-for="piece in player1Pieces" :key="piece.id"
        :disabled="currentTurn == gameStates.player1Turn ? false : true" class="drag px-2" :data="piece"
        @cut="remove(player1Pieces, piece)">
        <gamePiece :pieceColor="
          currentTurn == gameStates.player1Turn ? piece.pieceColor : 'gray'
        " :size="piece.size">
        </gamePiece>
      </drag>
    </transition-group>

    <!-- PLAYER 2 PIECES -->
    <transition-group name="list" tag="div" class="flex justify-center ">
      <drag v-for="piece in player2Pieces" :key="piece.id"
        :disabled="currentTurn == gameStates.player2Turn ? false : true" class="drag px-2" :data="piece"
        @cut="remove(player2Pieces, piece)">
        <gamePiece :pieceColor="
          currentTurn == gameStates.player2Turn ? piece.pieceColor : 'gray'
        " :size="piece.size">
        </gamePiece>
      </drag>
    </transition-group>
    <!-- RESET BUTTON -->
    <div class="flex  justify-center text-lg py-3">
      <button @click="resetBoard" type="button"
        class="inline-flex rounded-md border border-transparent bg-indigo-600 px-6 py-3 text-base font-medium text-white shadow-sm hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2">
        RESET
      </button>
    </div>
  </div>
</template>
  
<script setup lang="ts">
import { Drag, DropList } from "vue-easy-dnd";
import { ref } from "vue";
import gamePiece from "./game-piece.vue";
import rules from "./rules.vue";

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

class GameBoxData {
  constructor(
    public pieces: [],
    public containsWinningPiece: boolean
  ) {
    this.pieces = pieces;
    this.containsWinningPiece = containsWinningPiece;
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

const box1Data = ref(new GameBoxData([], false));
const box2Data = ref(new GameBoxData([], false));
const box3Data = ref(new GameBoxData([], false));
const box4Data = ref(new GameBoxData([], false));
const box5Data = ref(new GameBoxData([], false));
const box6Data = ref(new GameBoxData([], false));
const box7Data = ref(new GameBoxData([], false));
const box8Data = ref(new GameBoxData([], false));
const box9Data = ref(new GameBoxData([], false));

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

var board = [
  [box1Data, box2Data, box3Data],
  [box4Data, box5Data, box6Data],
  [box7Data, box8Data, box9Data],
];

const selected = ref([]);
const selectedList = ref(0);

const gameStates = {
  player1Turn: "Player 1",
  player2Turn: "Player 2",
  gameOver: "Game Over",
};

const winner = ref("");

const currentTurn = ref(gameStates.player1Turn);

function selection(item) {
  return selected.value.length > 0 ? selected : item;
}
/**
 * Inserts one or multiple selected items into the target
 * droplist
 *
 * @event {InsertEvent} - holds dragging data and drop index
 * @llistNameist String - name of the list in the data section
 */
function onInsert(event, boxIndex) {
  boxes.value[boxIndex].value.pieces.push(event.data as never)
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
  const index = selected.value.indexOf(item as never);
  if (index > -1) {
    selected.value.splice(index, 1);
  } else {
    selected.value.push(item as never);
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
  if (box.value.pieces == undefined || box.value.pieces.length === 0) {
    return true;
  } else if (p.size > box.value.pieces[box.value.pieces.length - 1].size) {
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

  //Horizontal
  for (let i = 0; i < 3; i++) {
    if (
      board[i][0].value.pieces.at(-1)?.pieceColor === playerColor &&
      board[i][1].value.pieces.at(-1)?.pieceColor === playerColor &&
      board[i][2].value.pieces.at(-1)?.pieceColor === playerColor
    ) {
      console.log("HORIZONTAL WINNER");
      board[i][0].value.containsWinningPiece = true;
      board[i][1].value.containsWinningPiece = true;
      board[i][2].value.containsWinningPiece = true;
      winner.value = playerColor;
      currentTurn.value = gameStates.gameOver;
      return true;
    }
  }

  //Vertical
  for (let i = 0; i < 3; i++) {
    if (
      board[0][i].value.pieces.at(-1)?.pieceColor === playerColor &&
      board[1][i].value.pieces.at(-1)?.pieceColor === playerColor &&
      board[2][i].value.pieces.at(-1)?.pieceColor === playerColor
    ) {
      console.log("VERTICAL WINNER");

      board[0][i].value.containsWinningPiece = true;
      board[1][i].value.containsWinningPiece = true;
      board[2][i].value.containsWinningPiece = true;

      winner.value = playerColor;
      currentTurn.value = gameStates.gameOver;
      return true;
    }
  }

  //Diagonals
  if (
    board[0][0].value.pieces.at(-1)?.pieceColor === playerColor &&
    board[1][1].value.pieces.at(-1)?.pieceColor === playerColor &&
    board[2][2].value.pieces.at(-1)?.pieceColor === playerColor
  ) {
    console.log("DIAGONAL WINNER");

    board[0][0].value.containsWinningPiece = true;
    board[1][1].value.containsWinningPiece = true;
    board[2][2].value.containsWinningPiece = true;

    winner.value = playerColor;
    currentTurn.value = gameStates.gameOver;
    return true;
  }
  if (
    board[0][2].value.pieces.at(-1)?.pieceColor === playerColor &&
    board[1][1].value.pieces.at(-1)?.pieceColor === playerColor &&
    board[2][0].value.pieces.at(-1)?.pieceColor === playerColor
  ) {
    console.log("DIAGONAL WINNER");

    board[0][2].value.containsWinningPiece = true;
    board[1][1].value.containsWinningPiece = true;
    board[2][0].value.containsWinningPiece = true;

    winner.value = playerColor;
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

function resetBoard() {
  //reset boxes
  boxes.value.forEach((b) => {
    b.value.pieces = [];
    b.value.containsWinningPiece = false;
  });

  //reset pieces
  player1Pieces.value = [
    new GamePieceData(1, "red", 1),
    new GamePieceData(2, "red", 1),
    new GamePieceData(3, "red", 2),
    new GamePieceData(4, "red", 2),
    new GamePieceData(5, "red", 3),
    new GamePieceData(6, "red", 3),
  ];

  player2Pieces.value = [
    new GamePieceData(7, "blue", 1),
    new GamePieceData(8, "blue", 1),
    new GamePieceData(9, "blue", 2),
    new GamePieceData(10, "blue", 2),
    new GamePieceData(11, "blue", 3),
    new GamePieceData(12, "blue", 3),
  ];

  //reset turn
  currentTurn.value = gameStates.player1Turn;

  //reset winner
  winner.value = "";
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
  background-color: #14532d;
}

.drop-forbidden {
  background-color: #881337;
}

.drop-in {
  box-shadow: 0 0 5px #1e3a8a;
}

.list-enter,
.list-leave-to {
  opacity: 0;
}

.list-leave-active {
  position: absolute;
}

.wrapper .list {
  border: 3px solid #D6D3D1;
  width: 200px;
  min-height: 200px;
  border-radius: 0.25rem;
}

.wrapper .list .item {
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
}


.wrapper .list .item.drag-image {
  background-color: #dcffdc;
  transform: translate(-50%, -50%);
}
</style>