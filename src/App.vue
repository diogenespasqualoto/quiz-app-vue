<script setup>
import { ref, computed } from 'vue'

const questions = ref([
  {
    question: 'What is the Vue JS?',
    answer: 0,
    options: [
      'A front end framework ',
      'A library',
      'An ice cream maker'
    ], selected: null
  },
  {
    question: 'What is Vuex?',
    answer: 1,
    options: [
      'Vue with an x ',
      'A cheese selection',
      'State Management library'
    ], selected: null
  },
  {
    question: 'What is Vue Router used for?',
    answer: 2,
    options: [
      'Walking in space',
      'AA routing librariry for vue JS',
      'Burger sauce',
      'Quizzes'
    ], selected: null
  },
])

const quizCompleted = ref(false)
const currentQuestion = ref(0)
const score = computed(() => {
  var value = 0
  questions.value.map(q => {
    if (q.selected == q.answer) {
      value++
    }
  })
  return value
})

const getCurrentQuestion = computed(() => {
  var question = questions.value[currentQuestion.value]
  question.index = currentQuestion.value
  return question
})

const setAnswer = evt => {
  questions.value[currentQuestion.value].selected = evt.target.value
  evt.target.value = null
}

const NextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++
  } else {
    quizCompleted = true
  }
}
</script>

<template>
  <main class="app">
    <h1>The quiz</h1>

    <section class="quiz">
      <div class="quiz-info">
        <span class="question">{{ getCurrentQuestion.question }}</span>
        <span class="score">score{{ score }}/{{ questions.length }}</span>
      </div>

      <div class="options">
        <label v-for="(option, index) in getCurrentQuestion.options" :key="index" :class="`option ${getCurrentQuestion.selected == index
        ? index == getCurrentQuestion.answer
          ? 'correct'
          : 'wrong'
        : ''
        } ${getCurrentQuestion.selected != null && index != getCurrentQuestion.selected
          ? 'disabled'
          : ''
        }`">
          <input type="radio" :name="getCurrentQuestion.index" :value="index" v-model="getCurrentQuestion.selected"
            @change="setAnswer">
          <span>{{ option }}</span>
        </label>
      </div>

    </section>
  </main>
</template>

<style >
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Montserrat', sans-serif;
}

body {
  background-color: #271C36;
  color: #FFF;
}
</style>
