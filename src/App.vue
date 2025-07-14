<script setup>
import { ref, computed } from 'vue';

const songs = ref(['First song', 'Second song', 'Third song', 'Fourth song', 'Fifth song']);
const isShuffling = ref(false);
const inputVal = ref(null);
const timer = ref(null);

const shuffleList = () => {
  let listCopy = songs.value;
  listCopy.sort(() => Math.random() - 0.5);
  songs.value = listCopy;
};

const mixingInterval = computed(() => inputVal.value * 1000);

const turnOnMixing = () => {
  clearInterval(timer.value);
  if (isShuffling.value) {
    timer.value = setInterval(() => {
      shuffleList();
    }, mixingInterval.value);
  }
};

const handleMixing = () => {
  if (inputVal.value <= 0) {
    alert('Incorrect interval');
    return;
  }
  isShuffling.value = !isShuffling.value;
  turnOnMixing();
};
</script>

<template>
  <ul class="songs-container flex flex-col gap-2 p-5 border-2 border-black">
    <li class="song-name" v-for="song in songs">
      {{ song }}
    </li>
  </ul>
  <input
    v-model="inputVal"
    :disabled="isShuffling"
    type="number"
    placeholder="Enter interval of mixes..."
    class="interval-input p-3 border-1 border-black hover:bg-gray-300 mr-5 disabled:hover:cursor-not-allowed disabled:bg-gray-300"
  />
  <button class="mix-switch p-3 border-1 border-black hover:bg-gray-300" @click="handleMixing">
    {{ isShuffling ? 'Stop mixing' : 'Start mixing' }}
  </button>
</template>
