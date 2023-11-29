<script>
import OneGudda from './OneGudda.vue'
import { wordList } from '../assets/wordList'

export default {
  props: {
    pickedWords: Array,
    selfDetails: {
      type: Object,
      required: true
    },
    opponent: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      wordMeanings: wordList,
      words: [],
      secondsRemaining: 15,
      timer: null,
      selectedWord: 0
    }
  },
  methods: {
    startTimer() {
      this.timer = setInterval(() => {
        if (this.secondsRemaining > 0) {
          this.secondsRemaining--
        } else {
          clearInterval(this.timer)
          this.$emit('showdownTime') // Emit the event when the time is up
        }
      }, 1000)
    }
  },
  mounted() {
    this.startTimer()
  },
  beforeUnmount() {
    clearInterval(this.timer)
  },
  components: { OneGudda }
}
</script>
<template>
  <div class="flex flex-col mt-32 items-center">
    <div class="text-white/70">00:{{ secondsRemaining }}</div>

    <div class="mt-8 text-white">Learn these 3 words</div>

    <div class="flex flex-row text-xl text-white gap-8 mt-12">
      <div
        class="cursor-pointer"
        @click="selectedWord = 0"
        :class="selectedWord == 0 ? 'underline' : ''"
      >
        {{ pickedWords[0] }}
      </div>
      <div
        class="cursor-pointer"
        @click="selectedWord = 1"
        :class="selectedWord == 1 ? 'underline' : ''"
      >
        {{ pickedWords[1] }}
      </div>
      <div
        class="cursor-pointer"
        @click="selectedWord = 2"
        :class="selectedWord == 2 ? 'underline' : ''"
      >
        {{ pickedWords[2] }}
      </div>
    </div>

    <div class="flex flex-col bg-white p-12 w-96 items-center mt-8 rounded-md text-center">
      <div class="text-2xl font-semibold mb-4">{{ pickedWords[selectedWord] }}</div>
      <div class="mb-8">{{ wordMeanings[pickedWords[selectedWord]].meaning }}</div>
      <div class="">
        {{ wordMeanings[pickedWords[selectedWord]].sentence }}
      </div>
    </div>

    <div class="flex flex-row gap-32">
      <OneGudda :name="selfDetails.name" :avatar="selfDetails.avatar" class="mt-24"></OneGudda>
      <OneGudda :name="opponent.name" :avatar="opponent.avatar" class="mt-24"></OneGudda>
    </div>
  </div>
</template>
