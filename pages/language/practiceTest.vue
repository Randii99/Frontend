<script setup lang="ts">
import { useRouter } from 'vue-router'
// Initial question count
const currentQuestion = ref(1)
const totalQuestions = ref(3) // Set the total number of questions
const answeredQuestions = ref(0)

// Array of questions
const questions = [
  "Reflect on the impact that storytellers like Mr. Thompson can have on the development of children's creativity and moral values. Provide specific examples from your own experiences or observations.",
  'In what ways do stories told by elders in a community contribute to the cultural heritage and identity of its members? Support your answer with examples from different cultures or societies.',
  'Discuss the role of storytelling in transmitting important life lessons and values across generations. Provide examples of stories from different cultures that illustrate this phenomenon.',
]

// Data property to store the answer
const answer = ref('')
const router = useRouter()
// Function to update the question counter and simulate moving to the next question
const nextQuestion = (): void => {
  if (currentQuestion.value < totalQuestions.value) {
    currentQuestion.value++
    answeredQuestions.value++
    // Reset the answer
    answer.value = ''
  } else {
    alert('You have completed all practice questions.')
    router.push('/language') // Navigate to the desired page
  }
}

// Computed property to calculate the progress bar width
const progressPercentage = computed(() => (answeredQuestions.value / totalQuestions.value) * 100)
const innerProgressBarWidth = computed(() => `${progressPercentage.value}%`)

// Get the current question text
const currentQuestionText = computed(() => questions[currentQuestion.value - 1])

// Computed property to check if all questions are done
const isAllQuestionsDone = computed(() => answeredQuestions.value === totalQuestions.value)
</script>

<template>
  <div class="language-Question-page">
    <!-- Header -->
    <div class="div">
      <NuxtLink to="/language">
        <img class="group-3" src="~/assets/images/cross.png" />
      </NuxtLink>
      <div class="text-wrapper-1">Practice Test</div>
    </div>

    <div class="panel" :class="{ 'panel-done': isAllQuestionsDone }">
      <!-- Inner progress bar -->
      <div class="inner-progress-bar" :style="{ width: innerProgressBarWidth }"></div>
    </div>

    <!-- Question counter -->
    <div class="text-wrapper-12">{{ currentQuestion }} / {{ totalQuestions }}</div>

    <!-- Question and Answer -->
    <div v-if="currentQuestion <= totalQuestions">
      <p class="text-wrapper-14">{{ currentQuestionText }}</p>
      <textarea
        v-model="answer"
        class="rectangle-9"
        name="w3review"
        rows="15"
        cols="70"
        placeholder="Write your answer here"
      ></textarea>
    </div>

    <div class="div-wrapper">
      <!-- Next Button -->
      <button type="button" class="text-wrapper-19" @click="nextQuestion">Next</button>
    </div>

    <!-- Completion Message -->
  </div>
</template>

<style scoped>
.language-Question-page {
  background-color: #ffffff;
  display: flex;
  flex-direction: row;
  justify-content: center;
  width: 1378px; /* Set the width */
  height: 740px; /* Set the height */
}

.language-Question-page .div {
  background: #fff;
  box-shadow: 0px 4px 4px 0px rgba(0, 0, 0, 0.25);
  width: 100%; /* This will make the child div fill the width of the parent */
  height: 100%; /* This will make the child div fill the height of the parent */
  position: relative;
  overflow: hidden;
}

.panel {
  position: absolute;
  top: 38px;
  left: 400px;
  width: 560px; /* Adjusted width for three questions */
  height: 10px;
  flex-shrink: 0;
  background: #40bf9c; /* Default panel color */
}

.panel-done {
  background: var(--eden, #247158); /* Color when all questions are completed */
}

.inner-progress-bar {
  position: absolute;
  top: 0;
  left: 0;
  width: calc(560px / 3 * var(--progressPercentage)); /* Calculate width based on progress percentage */
  height: 100%;
  background: var(--eden, #247158); /* Inner progress bar color */
}

.language-Question-page .group-3 {
  position: absolute;
  top: 49px;
  left: 73px;
  width: 17px;
  height: 17px;
  flex-shrink: 0;
}
.language-Question-page .group-4 {
  position: absolute;
  width: 24px;
  height: 24px;
  flex-shrink: 0;
  top: 30px;
  right: 270px;
}
.language-Question-page .text-wrapper-1 {
  position: absolute;
  top: 48px;
  left: 103px;
  letter-spacing: 0;
  line-height: 15.6px;
  white-space: nowrap;
  color: #6b7f99;
  font-family: 'Fira Sans';
  font-size: 17px;
  font-style: normal;
  font-weight: 400;
  line-height: 21.6px; /* 120% */
}
.language-Question-page .text-wrapper-12 {
  position: absolute;
  top: 51px;
  left: 400px;
  width: 213px;
  color: #6b7f99;
  font-family: 'Fira Sans';
  font-size: 15px;
  font-style: normal;
  font-weight: 400;
  line-height: 21.6px; /* 120% */
}
.language-Question-page .text-wrapper-13 {
  position: absolute;
  width: 213px;
  top: 34px;
  right: 20px;
  color: #6b7f99;
  font-family: 'Fira Sans';
  font-size: 15px;
  font-style: normal;
  font-weight: 400;
  line-height: 21.6px; /* 120% */
}
.language-Question-page .text-wrapper-14 {
  position: absolute;
  width: 654px;
  left: 350px;
  top: 100px;
  color: #2d3846;
  font-family: 'Fira Sans';
  font-size: 18px;
  font-style: normal;
  font-weight: 400;
  line-height: 24px; /* 150% */
}

.language-Question-page .text-wrapper-16 {
  position: absolute;
  top: 248px;
  left: 300px;
  width: 832px;
  color: #000;
  font-family: 'Fira Sans';
  font-size: 18px;
  font-style: normal;
  font-weight: 400;
  line-height: 23.5px; /* 187.5% */
}

.language-Question-page .overlap-2 {
  position: absolute;
  width: 1378px;
  height: 120px;
  flex-shrink: 0;
  top: 627px;
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0px 0px 8px #3e4f661a;
}

.language-Question-page .rectangle-9 {
  position: absolute;
  top: 208px;
  left: 350px;
  width: 787px;
  height: 264px;
  flex-shrink: 0;
  border-radius: 4px;
  background: rgba(217, 217, 217, 0.28);
}

.language-Question-page .text-wrapper-19 {
  position: absolute;
  width: 76px;
  top: 12px;
  right: 20px;
  color: #fff;
  font-family: Poppins;
  font-size: 16px;
  font-style: normal;
  font-weight: 700;
  line-height: 16px; /* 114.286% */
}

.language-Question-page .div-wrapper {
  position: absolute;
  top: 38px;
  right: 90px;
  width: 124px;
  height: 41px;
  flex-shrink: 0;
  background: var(--Tradewind, #6ab29b);
  box-shadow: 0px 8px 21px 0px rgba(0, 0, 0, 0.16);
}
</style>
