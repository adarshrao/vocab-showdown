<script>
import OneGudda from './OneGudda.vue'

import crowdSound from '../assets/sounds/crowdCheer.flac'

export default {
  props: {
    currentRound: Number,
    winner: {
      type: String,
      required: true
    },
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
    return {}
  },
  methods: {
    getWinner() {
      if (this.winner == 'self') {
        return this.selfDetails
      } else {
        return this.opponent
      }
    },
    getLoser() {
      if (this.winner == 'self') {
        return this.opponent
      } else {
        return this.selfDetails
      }
    },
    resetGame() {
      console.log('resetting Game')
      this.$emit('resetGame')
    }
  },
  components: { OneGudda },
  mounted() {
    new Audio(crowdSound).play()
  }
}
</script>

<template>
  <div class="flex flex-col items-center mt-72">
    <div class="text-white font-semibold mb-8">Round {{ currentRound }} Winner</div>

    <div class="flex flex-row items-center gap-4 text-white mb-32">
      <div class="text-4xl">{{ getWinner().name }}</div>
    </div>

    <div class="flex flex-row mt-24">
      <OneGudda :name="getWinner().name" :avatar="getWinner().avatar" class="mr-32"></OneGudda>

      <OneGudda
        class="pt-4"
        :state="'dead'"
        :name="getLoser().name"
        :avatar="getLoser().avatar"
      ></OneGudda>
    </div>

    <div
      class="text-center bg-white text-black mt-12 py-2 w-1/4 mx-auto font-bold rounded-md text-lg cursor-pointer"
      @click="resetGame"
    >
      Next Round
    </div>
  </div>
</template>
