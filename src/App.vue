<script setup>
import { ref, computed } from 'vue';

const songs = ref(['First song', 'Second song', 'Third song', 'Fourth song', 'Fifth song']);
const isMixingEnabled = ref(false)
const inputVal = ref(null);
const timer = ref(null)

const shuffleList = () => {
    const listCopy = songs.value;
    for (let i = listCopy.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [listCopy[i], listCopy[j]] = [listCopy[j], listCopy[i]];
    }
    songs.value = listCopy;
}

const turnOnMixing = computed(() => {
    clearInterval(timer.value)
    if (isMixingEnabled.value) {
    timer.value = setInterval(() => {
        shuffleList();
    }, inputVal.value * 1000)}
})

const enableDisableMixing = () => {
    isMixingEnabled.value = !isMixingEnabled.value;
    turnOnMixing.value;
}

</script>

<template>
    <div class="songs-container flex flex-col gap-2 p-5 border-2 border-black">
        <div class="song-name " v-for="(song, index) in songs">
        {{index + 1 + '. ' + song }}
        </div>
    </div>
        <input v-model="inputVal" type="number" placeholder="Enter interval of mixes..." class="interval-input p-3 border-1 border-black hover:bg-gray-300 mr-5" @change="turnOnMixing">
        <button class="mix-switch p-3 border-1 border-black hover:bg-gray-300" :class="inputVal < 1 ? 'pointer-events-none' : ''" @click="enableDisableMixing">Enable/disable mixing</button>
        <p class="info mt-5">
            {{ isMixingEnabled ? 'Mixing enabled...' : 'Mixing disabled...' }}
        </p>
</template>