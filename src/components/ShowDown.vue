<script>
import OneGudda from './OneGudda.vue'
import { wordList } from '../assets/wordList'

import correctSound from '../assets/sounds/correct.wav'
import wrongSound from '../assets/sounds/wrong.mp3'

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
      questionsOver: false,
      currentQuestion: 0,
      questions: [],
      selectedAnswer: null,
      wrongOptions: [],
      correctCount: 0
    }
  },
  methods: {
    answerClicked(answer) {
      if (this.selectedAnswer == null) {
        this.selectedAnswer = answer

        if (
          this.questions[this.currentQuestion].options[answer] ==
          this.questions[this.currentQuestion].questionDetails.correctAnswer
        ) {
          new Audio(correctSound).play()
          this.correctCount++
        } else {
          new Audio(wrongSound).play()
        }
      } else {
        console.log('cant change answer after selection.')
      }
    },
    checkAnswer(answer) {
      if (
        this.questions[this.currentQuestion].options[answer] ==
        this.questions[this.currentQuestion].questionDetails.correctAnswer
      ) {
        console.log('Correct')
        return true
      } else {
        console.log('Wrong!')
        return false
      }
    },
    checkStatus(currentOptionNumber) {
      if (this.selectedAnswer == null) {
        return ' bg-[#8854C0] opacity-100'
      } else {
        if (currentOptionNumber == this.selectedAnswer) {
          if (this.checkAnswer(currentOptionNumber)) {
            return 'bg-[#01AD71]' // green
          } else {
            return 'bg-[#D9003B]' // red
          }
        } else if (this.checkAnswer(currentOptionNumber)) {
          return 'bg-[#01AD71]' // green
        } else {
          return ' bg-[#8854C0] opacity-50' // faded purple
        }
      }
    },
    nextQuestion() {
      if (this.currentQuestion < this.questions.length - 1) {
        this.currentQuestion += 1
        this.selectedAnswer = null
      } else {
        console.log('All Questions Over')
        this.questionsOver = true
      }
    },
    revealWinner() {
      console.log('emitting winner')

      let winner = ''

      if (this.correctCount == 3) {
        winner = 'self'
      } else {
        if (Math.floor(Math.random() * 2 > 0)) {
          winner = 'opponent'
        }
      }

      this.$emit('revealWinner', winner)
    }
  },
  components: { OneGudda },
  beforeMount() {
    // generate the questions array

    // console.log('This is running')

    for (let n = 0; n < this.pickedWords.length; n++) {
      let wrongOptions = []

      //
      while (wrongOptions.length <= 2) {
        console.log(Math.random() * Object.keys(wordList).length)

        let word = Object.keys(wordList)[Math.floor(Math.random() * Object.keys(wordList).length)]

        // console.log(word)
        // console.log(!wrongOptions.includes(word))
        // console.log(this.pickedWords[n] != word)

        let meaning = wordList[word].meaning

        if (!wrongOptions.includes(meaning) && this.pickedWords[n] != word) {
          wrongOptions.push(meaning)
        }
      }

      let unShuffledOptions = [...wrongOptions, wordList[this.pickedWords[n]].meaning]

      // shuffle the array

      let array = unShuffledOptions

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

      let shuffledOptions = array

      this.questions.push({
        questionDetails: {
          questionText: `What is the meaning of "${this.pickedWords[n]}"?`,
          correctAnswer: wordList[this.pickedWords[n]].meaning
        },
        options: shuffledOptions
      })
    }

    console.log(this.questions)
  }
}
</script>

<template>
  <div class="flex flex-col items-center mt-32">
    <div class="text-2xl text-white font-semibold mb-12">SHOWDOWN TIME</div>
    <div
      v-if="questionsOver == false"
      class="bg-[#461A42] flex flex-col py-8 p-12 rounded-xl align-top text-white text-xl text-center w-3/5"
    >
      <div class="mb-8">{{ questions[currentQuestion].questionDetails.questionText }}</div>

      <div class="flex flex-col gap-4 text-lg">
        <div
          :class="checkStatus(0)"
          @click="answerClicked(0)"
          class="py-4 rounded-md px-4 cursor-pointer"
        >
          {{ questions[currentQuestion].options[0] }}
        </div>
        <div
          :class="checkStatus(1)"
          @click="answerClicked(1)"
          class="py-4 rounded-md px-4 cursor-pointer"
        >
          {{ questions[currentQuestion].options[1] }}
        </div>
        <div
          :class="checkStatus(2)"
          @click="answerClicked(2)"
          class="py-4 rounded-md px-4 cursor-pointer"
        >
          {{ questions[currentQuestion].options[2] }}
        </div>
        <div
          :class="checkStatus(3)"
          @click="answerClicked(3)"
          class="py-4 rounded-md px-4 cursor-pointer"
        >
          {{ questions[currentQuestion].options[3] }}
        </div>
      </div>

      <div
        v-if="selectedAnswer != null && questionsOver == false"
        class="bg-white text-black mt-4 py-2 w-1/2 mx-auto font-bold rounded-md text-lg cursor-pointer"
        @click="nextQuestion"
      >
        Next Question
      </div>
    </div>
    <div
      v-if="questionsOver == true"
      class="bg-[#461A42] flex flex-col py-8 p-12 rounded-xl align-top text-white text-xl text-center w-3/5"
    >
      <div class="flex flex-col">
        <div class="text-3xl font-bold mt-12 mb-24">You got {{ correctCount }}/3 Correct</div>
        <!-- <div class="text-lg opacity-60 mb-8">
          Waiting for {{ opponent.name }} to finish answering ...
        </div> -->
        <div @click="revealWinner" class="cursor-pointer bg-black py-4 rounded-md">
          Reveal Winner
        </div>
      </div>
    </div>
    <div class="flex flex-row mt-16">
      <OneGudda :name="selfDetails.name" :avatar="selfDetails.avatar" class="mr-96"></OneGudda>
      <OneGudda :name="opponent.name" :avatar="opponent.avatar"></OneGudda>
    </div>
  </div>
</template>
