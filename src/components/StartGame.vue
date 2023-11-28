<script>
export default {
  data() {
    return { countDown: 4, intervalId: null }
  },
  methods: {
    decreaseCount() {
      if (this.countDown > 0) {
        this.countDown--
        console.log('Count: ' + this.countDown)
      } else {
        this.stopInterval()
        console.log('Count reached 0. Interval stopped.')
      }
    },
    startInterval() {
      this.intervalId = setInterval(this.decreaseCount, 1000)
    },
    stopInterval() {
      clearInterval(this.intervalId)
      console.log('emitting this')
      this.$emit('customEventName')
    }
  },
  mounted() {
    console.log('Start Countdown')
    this.countDown += -1
    this.startInterval()
  }
}
</script>

<template>
  <div class="flex flex-col items-center">
    <div class="text-white">Game Starting in</div>
    <div class="text-7xl text-white">{{ countDown }}</div>
  </div>
</template>
