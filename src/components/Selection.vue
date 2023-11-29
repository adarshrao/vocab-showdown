<script>
import playerSelectAudio from '../assets/sounds/playerSelect.wav'

export default {
  props: {
    selfDetails: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      remainingRunCount: 3 + Math.floor(Math.random() * 4),
      intervalId: null,
      players: [
        {
          name: 'Adarsh',
          avatar: '1',
          selected: true
        },
        {
          name: 'Jahangir',
          avatar: '2',
          selected: false
        },
        {
          name: 'Abigail',
          avatar: '4',
          selected: false
        }
      ]
    }
  },
  mounted() {
    this.intervalId = setInterval(this.updateSelected, 500)
  },
  methods: {
    updateSelected() {
      // This function iterates through

      let audio = new Audio(playerSelectAudio)
      audio.play()

      console.log(this.players)

      this.remainingRunCount--

      for (let n = 0; n < this.players.length; n++) {
        if (this.players[n].selected == true) {
          this.players[n].selected = false
          let nextIndex = (n + 1) % this.players.length
          this.players[nextIndex].selected = true

          break
        }
      }

      if (this.remainingRunCount < 1) {
        setTimeout(this.stopInterval(), 1000)
      }
    },
    stopInterval() {
      clearInterval(this.intervalId)

      let selectedPlayer = null

      for (let n = 0; n < this.players.length; n++) {
        if (this.players[n].selected == true) {
          selectedPlayer = (n + 2) % this.players.length
        }
      }

      // Okay so I'm not sure why but I'm having to do the above to actually get the selected player.
      // Pretty sure this will come back to bite me later. Again made it +2 to actually correspond to what's being shown on the UI.
      // Not a 100% sure but let's fix later

      console.log(`selected opponent is: ${this.players[selectedPlayer].name}`)

      setTimeout(this.$emit('opponentSelected', this.players[selectedPlayer].name), 4000)

      // changing the time here hasn't been helping, unclear why
    }
  }
}
</script>

<template>
  <div class="h-screen">
    <div class="w-full flex justify-center flex-col items-center">
      <div class="text-3xl font-semibold mt-72 mb-12 text-white">Selecting your opponent</div>
    </div>

    <div class="flex flex-row mx-auto justify-center mt-64 items-center">
      <div class="flex flex-col items-center mr-32">
        <div class="px-2 py-1 mb-2 bg-black/40 rounded-md text-white">{{ selfDetails.name }}</div>
        <img class="w-max" :src="'src/assets/guddas/' + selfDetails.avatar + '.png'" alt="" />
      </div>

      <div class="flex justify-center flex-col items-center">
        <div class="flex flex-row gap-8">
          <div v-for="item in players">
            <div
              class="flex flex-col items-center"
              :class="item.selected == true ? 'opacity-100' : 'opacity-50'"
            >
              <div class="px-2 py-1 mb-2 bg-black/40 rounded-md text-white">
                {{ item.name }}
              </div>
              <img class="w-max" :src="'src/assets/guddas/' + item.avatar + '.png'" alt="" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
