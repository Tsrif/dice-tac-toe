<template>
  <transition-group name="list" tag="div" class="grid grid-cols-3 gap-3">
    <drag
      v-for="piece in player1Pieces"
      :key="piece.id"
      class="drag"
      :data="piece"
      @cut="remove(player1Pieces, piece)"
    >
      <gamePiece :pieceColor="piece.pieceColor" :size="piece.size"> </gamePiece>
    </drag>
  </transition-group>
  <v-content>
    <v-container fluid class="wrapper">
      <v-row>
        <v-col>
          <drop-list
            :items="box1Data"
            class="list"
            @insert="onInsert($event, 'box1Data')"
            @reorder="$event.apply(box1Data)"
            mode="cut"
          >
            <template v-slot:item="{ item }">
              <drag
                class="item"
                :class="{ selected: selected.indexOf(item) > -1 }"
                @click="toggleSelected(box1Data, item)"
                @cut="remove(box1Data, item)"
                :data="selection(item)"
                :key="item.id"
              >
                <gamePiece :pieceColor="item.pieceColor" :size="item.size">
                </gamePiece>
              </drag>
            </template>
            <template v-slot:feedback="{ data }">
              <template v-if="selected.length > 0">
                <div v-for="f in data" class="item feedback" :key="f.id">
                  {{ f.id }}
                </div>
              </template>
              <template v-else>
                <div class="item feedback" :key="data.id">
                  {{ data.id }}
                </div>
              </template>
            </template>
          </drop-list>
        </v-col>
        <v-col>
          <drop-list
            :items="player2Pieces"
            class="list"
            @insert="onInsert($event, 'player2Pieces')"
            @reorder="$event.apply(player2Pieces)"
            mode="cut"
          >
            <template v-slot:item="{ item }">
              <drag
                class="item"
                :class="{ selected: selected.indexOf(item) > -1 }"
                @click="toggleSelected(player2Pieces, item)"
                @cut="remove(player2Pieces, item)"
                :data="selection(item)"
                :key="item.id"
              >
                <gamePiece :pieceColor="item.pieceColor" :size="item.size">
                </gamePiece>
              </drag>
            </template>
            <template v-slot:feedback="{ data }">
              <template v-if="selected.length > 0">
                <div v-for="f in data" class="item feedback" :key="f.id">
                  {{ f.id }}
                </div>
              </template>
              <template v-else>
                <div class="item feedback" :key="data.id">
                  {{ data.id }}
                </div>
              </template>
            </template>
          </drop-list>
        </v-col>
      </v-row>
    </v-container>
  </v-content>
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

const items1 = ref([
  {
    name: "Frozen Yogurt",
    calories: 159,
    fat: 6.0,
  },
  {
    name: "Ice cream sandwich",
    calories: 237,
    fat: 9.0,
  },
  {
    name: "Eclair",
    calories: 262,
    fat: 16.0,
  },
  {
    name: "More ice cream sandwich",
    calories: 237,
    fat: 9.0,
  },
]);
const items2 = ref([]);
const selected = ref([]);
const selectedList = ref(0);

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
  if (event.data.length > 0) {
    event.data.forEach((e, idx) => {
      // event.index is the starting point of the target droplist
      // event.index + idx = appending the items one after the other
      this[listName].splice(event.index + idx, 0, e);
    });
  } else {
    // here we have just one item
    // @see https://codesandbox.io/s/droplist-ozs8b
    this[listName].splice(event.index, 0, event.data);
  }

  this.selected = [];
}
function remove(array, value) {
  // Following logic is taken from https://codesandbox.io/s/easy-dnd-demo-9mbij
  // In addition if we have some items in the selection
  // we apply the same logic just with a loop

  if (this.selected.length > 0) {
    this.selected.forEach((e) => {
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
  if (listName !== this.selectedList) {
    this.selected = [];
    this.selectedList = listName;
  }

  // Basic toggeling logic
  // If an item is in the list remove it
  // otherwise add it to the list
  const index = this.selected.indexOf(item);
  if (index > -1) {
    this.selected.splice(index, 1);
  } else {
    this.selected.push(item);
  }
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
  border: 1px solid black;
  margin: 100px auto;
  width: 200px;
  min-height: 200px;
}
.wrapper .list .item {
  padding: 20px;
  margin: 10px;
  background-color: #131342;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
}
.wrapper .list .item.selected {
  border: 2px solid red;
}
.wrapper .list .item.feedback {
  background-color: #ffdcdc;
  border: 2px dashed black;
}
.wrapper .list .item.drag-image {
  background-color: #dcffdc;
  transform: translate(-50%, -50%);
}
</style>