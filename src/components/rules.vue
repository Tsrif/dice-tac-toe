<template>
  <div>
    <button
      class="text-white active:bg-gray-500 font-bold uppercase text-sm px-6 py-3 rounded hover:bg-gray-500 outline-none focus:outline-none mr-1 mb-1 ease-linear transition-all duration-150"
      type="button" @click="toggleModal()">
      <font-awesome-icon icon="fa-solid fa-question" size="1x" />
    </button>
    <div v-if="showModal"
      class="overflow-x-hidden overflow-y-auto fixed inset-0 z-50 outline-none focus:outline-none justify-center items-center flex">
      <div class="relative w-auto my-6 mx-auto max-w-6xl">
        <!--content-->
        <div
          class="border-0 rounded-lg shadow-lg relative flex flex-col w-full bg-[#111827] outline-none focus:outline-none">
          <!--body-->
          <div class="relative p-6 flex-auto">
            <p class="my-4 text-white text-lg leading-relaxed">
              Like Tic-Tac-Toe, you must be the first player to get 3 pieces in
              a row to win.
              <br />
              When it is your turn you can either:
              <br />
              <br />

              <b>1. </b>
              Put a new die on the board, on an empty space or over a die
              with a lower value
              <br />
              <i>(ex. a die with a 3 on it will be able to cover a die with a 2
                or 1 on it)</i>.
              <br />
              <br />
              <b>2. </b>
              Move one of your dice already on the board to an empty space or
              over a die with a lower value.
              <br />
              <br />

              Spaces will turn <b class="text-[#14532d]">green</b> when you are
              able to move a die to them
              <br />
              Spaces will turn <b class="text-[#881337]">red</b> when you are
              not able to move a die to them.
            </p>
          </div>
          <!--footer-->
          <div class="flex items-center justify-center p-4 border-t border-solid border-slate-800 rounded-b">
            <button
              class="text-rose-500 bg-transparent border border-solid border-rose-500 hover:bg-rose-500 hover:text-white active:bg-rose-600 font-bold uppercase text-sm px-6 py-3 rounded outline-none focus:outline-none mr-1 mb-1 ease-linear transition-all duration-150"
              type="button" @click="toggleModal()">
              Close
            </button>
          </div>
        </div>
      </div>
    </div>
    <div v-if="showModal" class="opacity-25 fixed inset-0 z-40 bg-black"></div>
  </div>
</template>


<script setup lang="ts">
import { computed, ref } from "vue";

const showModal = ref(false);
function toggleModal() {
  console.log(showModal.value);
  showModal.value = !showModal.value;
}
</script>