<script>
import OneGudda from './OneGudda.vue'

import fightAudio from '../assets/sounds/fight.wav'

export default {
  props: {
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
  components: { OneGudda },

  mounted() {
    setTimeout(() => {
      let audio = new Audio(fightAudio)
      audio.play()
    }, 1000)

    setTimeout(() => {
      this.$emit('startRound')
    }, 3500)
  }
}
</script>

<template>
  <div class="flex flex-col items-center mt-72">
    <div class="text-white font-semibold mb-8">Round 1</div>

    <div class="flex flex-row items-center gap-4 text-white mb-32">
      <div class="text-4xl">{{ selfDetails.name }}</div>
      <div class="text-2xl">vs</div>
      <div class="text-4xl">{{ opponent.name }}</div>
    </div>

    <div class="flex flex-row mt-24">
      <OneGudda :name="selfDetails.name" :avatar="selfDetails.avatar" class="mr-48"></OneGudda>
      <OneGudda :name="opponent.name" :avatar="opponent.avatar"></OneGudda>
    </div>
  </div>
</template>
