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
      questionsOver: false,
      currentQuestion: 0,
      questions: [
        {
          questionDetails: {
            questionText: 'What is the meaning of "Subsequent"?',
            correctAnswer: 'following in time or order'
          },
          options: ['blah', 'blah blah', 'blah blah blah', 'following in time or order']
        },
        {
          questionDetails: {
            questionText: 'What is the meaning of "illicit"?',
            correctAnswer: 'following in time or order'
          },
          options: ['blah', 'blah blah', 'blah blah blah', 'following in time or order']
        },
        {
          questionDetails: {
            questionText: 'What is the meaning of "Neanderthal"?',
            correctAnswer: 'following in time or order'
          },
          options: ['blah', 'blah blah', 'blah blah blah', 'following in time or order']
        }
      ],
      questionDetails: {
        questionText: 'What is the meaning of "Subsequent"?',
        correctAnswer: 'following in time or order'
      },
      options: ['blah', 'blah blah', 'blah blah blah', 'following in time or order'],
      selectedAnswer: null
    }
  },
  methods: {
    formQuestion(word) {
      return `What is the meaning of "${word}"?`
    },
    checkAnswer(answer) {
      this.selectedAnswer = answer

      if (this.options[answer] == this.questionDetails.correctAnswer) {
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
            return 'bg-[#01AD71]'
          } else {
            return 'bg-[#D9003B]'
          }
        } else return ' bg-[#8854C0] opacity-50'
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
      this.$emit('revealWinner', 'self')
    }
  },
  components: { OneGudda }
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

      <div class="flex flex-col gap-4">
        <div
          :class="checkStatus(0)"
          @click="checkAnswer(0)"
          class="py-4 rounded-md text-lg cursor-pointer"
        >
          {{ questions[currentQuestion].options[0] }}
        </div>
        <div
          :class="checkStatus(1)"
          @click="checkAnswer(1)"
          class="py-4 rounded-md text-lg cursor-pointer"
        >
          {{ questions[currentQuestion].options[1] }}
        </div>
        <div
          :class="checkStatus(2)"
          @click="checkAnswer(2)"
          class="py-4 rounded-md text-lg cursor-pointer"
        >
          {{ questions[currentQuestion].options[2] }}
        </div>
        <div
          :class="checkStatus(3)"
          @click="checkAnswer(3)"
          class="py-4 rounded-md text-lg cursor-pointer"
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
        <div class="text-3xl font-bold mb-24">You got 3/3 Correct</div>
        <div class="text-lg opacity-60 mb-8">
          Waiting for {{ opponent.name }} to finish answering to reveal winner...
        </div>
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
