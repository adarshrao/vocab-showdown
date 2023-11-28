<script>
import OneGudda from './OneGudda.vue'

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
    return {
      // selfDetails: {
      //   name: 'Adarsh',
      //   avatar: `1`
      // },
      // opponent: {
      //   name: 'Abigail',
      //   avatar: `2`
      // }
    }
  },
  components: { OneGudda },

  mounted() {
    setTimeout(() => {
      this.$emit('startRound')
    }, 1000)
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
