<script>
import Selection from './components/Selection.vue'
import OneGudda from './components/OneGudda.vue'
import StartGame from './components/StartGame.vue'
import RoundStart from './components/RoundStart.vue'
import VsScreen from './components/VsScreen.vue'
import LearnWords from './components/LearnWords.vue'
import ShowDown from './components/ShowDown.vue'
import RoundWinner from './components/RoundWinner.vue'

import { wordList } from './assets/wordList'

// fetch length of wordList
// pick 3 random words from it with no repetitions (or just shuffle it and pick the first 3)
// then, replace them with the round's words
// need to ensure that the selection happens at round start, not earlier or later

// when the round starts, I need to listen to an event that does all this shenanigans.

export default {
  data() {
    return {
      gameState: 'vsScreen',
      // lobby, waiting, starting, round, selection, vsScreen, learnWords, showdownTime
      selfDetails: {
        name: 'Shardo',
        avatar: '5'
      },
      pickedWords: [],
      winner: '',
      opponent: {
        name: 'Jahangir',
        avatar: '2'
      },
      players: [
        {
          name: 'Adarsh',
          avatar: '1'
        },
        {
          name: 'Jahangir',
          avatar: '2'
        },
        {
          name: 'Abigail',
          avatar: '4'
        }
      ]
    }
  },
  methods: {
    switchState(value) {
      console.log(`Transitioning from ${this.gameState} to ${value}`)
      this.gameState = value
    },
    addPlayer() {
      this.players.push({
        name: this.selfDetails.name,
        avatar: '5'
      })
      this.switchState('starting')
    },
    testEvent() {
      console.log('Event received')
    },
    opponentSelected(name) {
      for (let n = 0; n < this.players.length; n++) {
        if (this.players[n].name == name) {
          this.opponent = this.players[n]
        }
      }

      this.gameState = 'vsScreen'
    },
    handleWinner(player) {
      console.log(player)
      this.winner = player

      this.switchState('revealWinner')
    },
    selectThreeWords() {
      let words = Object.keys(wordList)

      let array = words

      let currentIndex = array.length
      let randomIndex

      // While there remain elements to shuffle...
      while (currentIndex !== 0) {
        // Pick a remaining element...
        randomIndex = Math.floor(Math.random() * currentIndex)
        currentIndex--

        // And swap it with the current element.
        ;[array[currentIndex], array[randomIndex]] = [array[randomIndex], array[currentIndex]]
      }

      this.pickedWords = array.splice(0, 3)

      // let me just send the words and let the picking happen later?

      // let justWords = array.splice(0, 3)

      // for (let n = 0; n < justWords.length; n++) {
      //   this.pickedWords.push(wordList[justWords[n]])
      // }

      console.log(this.pickedWords)
    }
  },
  components: {
    Selection,
    OneGudda,
    StartGame,
    RoundStart,
    VsScreen,
    LearnWords,
    ShowDown,
    RoundWinner
  },
  computed: {
    showTitle() {
      if (this.gameState == 'lobby') {
        return true
      } else if (this.gameState == 'waiting') {
        return true
      } else if (this.gameState == 'starting') {
        return true
      } else {
        return false
      }
    },
    showPlayerList() {
      if (this.gameState == 'lobby') {
        return true
      } else if (this.gameState == 'waiting') {
        return true
      } else if (this.gameState == 'starting') {
        return true
      } else {
        return false
      }
    }
  }
}
</script>

<template>
  <div class="bg-[url('../assets/ring.png')] bg-no-repeat bg-center bg-gray-900 h-screen">
    <!-- <img class="absolute" src="./assets/ring.png" alt="" /> -->
    <div class="w-full flex justify-center flex-col items-center">
      <div v-if="showTitle" class="text-4xl font-semibold mt-48 mb-12 text-white">
        Vocabulary Showdown
      </div>

      <div v-if="gameState == 'lobby'">
        <input
          class="border w-max text-center py-2 rounded-md mb-4"
          type="text"
          v-model="selfDetails.name"
          placeholder="Enter your Name"
        />
        <div
          class="bg-white text-center font-semibold rounded-md py-2 px-4 cursor-pointer border w-full px-2"
          @click="addPlayer()"
        >
          Enter the Ring
        </div>
      </div>
      <div v-if="gameState == 'waiting'">
        <p class="text-white">Waiting for the teacher to start the game...</p>
      </div>

      <StartGame v-if="gameState == 'starting'" @customEventName="switchState('round')"></StartGame>
      <RoundStart
        class="mt-32"
        v-if="gameState == 'round'"
        @customEventName="switchState('selection')"
      ></RoundStart>
    </div>

    <div v-if="showPlayerList" class="w-full flex justify-center flex-col items-center">
      <div class="flex flex-row gap-8 mt-72">
        <div v-for="item in players">
          <OneGudda :key="item.name" :name="item.name" :avatar="item.avatar.toString()"></OneGudda>
        </div>
      </div>
    </div>

    <Selection
      :selfDetails="selfDetails"
      v-if="gameState == 'selection'"
      @opponentSelected="
        (n) => {
          opponentSelected(n)
        }
      "
    ></Selection>

    <VsScreen
      :selfDetails="selfDetails"
      :opponent="opponent"
      @startRound="
        () => {
          selectThreeWords()
          switchState('learnWords')
        }
      "
      v-if="gameState == 'vsScreen'"
    ></VsScreen>

    <LearnWords
      :pickedWords="pickedWords"
      :selfDetails="selfDetails"
      :opponent="opponent"
      v-if="gameState == 'learnWords'"
      @showdownTime="switchState('showdownTime')"
    ></LearnWords>

    <ShowDown
      :pickedWords="pickedWords"
      :selfDetails="selfDetails"
      :opponent="opponent"
      v-if="gameState == 'showdownTime'"
      @revealWinner="handleWinner"
    ></ShowDown>

    <RoundWinner
      :selfDetails="selfDetails"
      :opponent="opponent"
      :winner="winner"
      v-if="gameState == 'revealWinner'"
      @resetGame="switchState('selection')"
    ></RoundWinner>
  </div>
</template>

<style scoped></style>
