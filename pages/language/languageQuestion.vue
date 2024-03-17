<script setup lang="ts">
import { ref, computed } from 'vue'
const totalTime = 120 * 60 * 1000 // 2 hours in milliseconds
const isTimerHighlighted = computed(() => remainingTime <= 60 * 1000); // Check if remaining time is 1 minute or less

const router = useRouter()
// Set the total number of questions and time per question
const totalQuestions = ref(10)

// Sample question list received from backend
const questionList = [
  {
    id: 1,
    text: 'Rich countries often give money to poorer countries, but it does not solve poverty. Therefore, developed countries should give other types of help to the poor countries rather than financial aid. To what extent do you agree or disagree?',
  },
  {
    id: 2,
    text: "Write about the following topic. Some people believe that teaching children at home is best for a child's development while others think that it is important for children to go to school. Discuss the advantages of both methods and give your own opinion. Give reasons for your answer and include any relevant examples from your own knowledge or experience.",
  },
  {
    id: 3,
    text: 'Reporting of crimes and other kinds of violent news on television and in newspapers can have adverse consequences. This kind of information should be restricted from being shown in the media. To what extent do you agree or disagree with this statement?',
  },
  {
    id: 4,
    text: 'Many people think modern communication technology is having some negative effects on social relationships. Do you agree or disagree?',
  },
  {
    id: 5,
    text: 'Some people say free time activities for children should be organized by parents. Others say that children should be free to choose what they do in their free time. Discuss both views and give your opinion.',
  },
  {
    id: 6,
    text: 'In some countries young people are encouraged to work or travel for a year between finishing high school and starting university studies. Discuss the advantages and disadvantages for young people who decide to do this .',
  },
  {
    id: 7,
    text: 'The advantages of the spread of English as a global language will continue to outweigh its disadvantages. To what extent do you agree or disagree?',
  },
  {
    id: 8,
    text: 'Write about the following topic. To meet the growing need for food to support an increasing population, a country should make use of edible insects as a food source. However, some people believe that insects are not only unhealthy but harvesting them will also negatively affect nature. What are the benefits and drawbacks of eating insects? Give reasons for your answer and include any relevant examples from your own knowledge or experience.',
  },
  {
    id: 9,
    text: 'Write about the following topic. Although there are a lot of translation software available, learning a language could still be advantageous. To what extent do you agree or disagree? Give reasons for your answer and include any relevant examples from your own knowledge or experience.',
  },
  {
    id: 10,
    text: 'Write about the following topic. Some people prefer to spend their lives doing the same things and avoiding change. Others, however, think that change is always a good thing. Discuss both these views and give your own opinion. Give reasons for your answer and include any relevant examples from your own knowledge or experience.',
  },
]

// Initialize question-specific variables
const currentQuestion = ref(1)
const answeredQuestions = ref(0)
const userAnswers = ref(Array(totalQuestions.value).fill(''))

// Variable to store the start time of each question
let questionStartTime = Date.now()
// Variable to store the current answer
const currentAnswer = ref('')

// Function to update the question counter and simulate moving to the next question
const nextQuestion = (): void => {
  saveAnswerAndTime()
  //console.log(userAnswers),
  //console.log(currentQuestion),
  // console.log()

  if (currentQuestion.value < totalQuestions.value) {
    console.log(currentQuestion.value)
    userAnswers.value[currentQuestion.value - 1] = currentAnswer.value.trim()
    currentQuestion.value = currentQuestion.value + 1
    answeredQuestions.value = answeredQuestions.value + 1

    remainingTime = 12 * 60 * 1000

    questionStartTime = Date.now() // Reset question start time

    // Clear the current answer for the next question
    currentAnswer.value = ''
  }
}

// Function to save the current answer and time duration for the question
const saveAnswerAndTime = (): void => {
  const elapsedTime = Date.now() - questionStartTime
  const currentQuestionIndex = currentQuestion.value - 1
  userAnswers.value[currentQuestionIndex] = currentAnswer.value.trim()

  // Send the response to the backend
  sendResponse(currentQuestionIndex, userAnswers.value[currentQuestionIndex], elapsedTime)
}

// Function to send user response to backend
const sendResponse = (questionIndex: number, userAnswer: string, durationInMilliseconds: number): void => {
  const question = questionList[questionIndex]

  // Convert duration from milliseconds to minutes
  const durationInMinutes = durationInMilliseconds / (1000 * 60)

  const data = {
    questionId: question.id,
    userAnswer: userAnswer,
    duration: durationInMinutes, // Use duration in minutes
  }

  // Print the data to console for debugging
  console.log('Question ID:', question.id)
  console.log('User Answer:', userAnswer)
  console.log('Duration (minutes):', durationInMinutes)

  // Replace 'your-backend-url' with the actual URL of your backend endpoint
  const backendUrl = 'your-backend-url'

  // Assuming you're using fetch API to send data to the backend
  fetch(backendUrl, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(data),
  })
    .then((response) => {
      // Handle response from the backend
      if (response.ok) {
        console.log('Response received from backend:', response)
      } else {
        throw new Error('Failed to send data to backend')
      }
    })
    .catch((error) => {
      // Handle error
      console.error('Error sending data to backend:', error)
    })
}

const formatTime = (time: number): string => {
  const minutes = Math.floor(time / (60 * 1000))
  const seconds = Math.floor((time % (60 * 1000)) / 1000)
  return `${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`
}

// Initialize remaining time for the first question
let remainingTime = 12 * 60 * 1000 // 12 minutes in milliseconds

const updateTimer = (): void => {
  remainingTime -= 1000
  // Display timer on the webpage
  const timerElement = document.getElementById('timer')
  if (timerElement) {
    timerElement.innerText = formatTime(remainingTime)
  }

  if (remainingTime <= 0) {
    nextQuestion()
    remainingTime = 12 * 60 * 1000 // Reset remaining time for the next question
  }
}
setInterval(updateTimer, 1000)
// Computed property to calculate the progress bar width
computed(() => {
  const progressPercentage = (answeredQuestions.value / totalQuestions.value) * 100
  return `${progressPercentage}%`
})

// Computed property to calculate the inner progress bar width
const innerProgressBarWidth = computed(() => {
  const innerProgressPercentage = (currentQuestion.value / totalQuestions.value) * 100
  return `${innerProgressPercentage}%`
})
const submitTest = (): void => {
  alert('Test submitted successfully!') // Example alert message
  router.push('/course')
}
// Computed property to check if all questions are done
const isAllQuestionsDone = computed(() => answeredQuestions.value === totalQuestions.value)
</script>

<template>
  <div class="language-Question-page">
    <div class="div">
      <img class="group-2" src="~/assets/images/timer.png" />
      <NuxtLink to="/language">
        <img class="group-3" src="~/assets/images/cross.png" />
      </NuxtLink>

      <!-- Timer display -->
      <div class="timer-container">
        <span :class="{ 'highlighted': isTimerHighlighted }" id="timer">{{ formatTime(remainingTime) }}</span>
      </div>

      <div class="text-wrapper-11">English Proficiency Test</div>
    </div>
    <!-- Panel -->
    <div class="panel" :class="{ 'panel-done': isAllQuestionsDone }">
      <!-- Inner progress bar -->
      <div class="inner-progress-bar" :style="{ width: innerProgressBarWidth }"></div>
    </div>
    <!-- Question counter -->
    <div class="text-wrapper-12">{{ currentQuestion + ' / ' + totalQuestions }}</div>
    <div class="text-wrapper-14">Read the following passage and answer the question .</div>
    <!-- Display current question text -->
    <div class="text-wrapper-15">
      {{ questionList[currentQuestion - 1].text }}
    </div>
    <textarea
      v-model="currentAnswer"
      class="rectangle-9"
      rows="15"
      cols="70"
      placeholder="Write your answer here"
    ></textarea>

    <div class="overlap-2"></div>
    <div class="div-wrapper">
      <!-- Link to the next question -->

      <button v-if="currentQuestion < totalQuestions" type="button" class="text-wrapper-19" @click="nextQuestion">
        Next
      </button>
      <button v-else type="button" class="text-wrapper-19" @click="submitTest">Submit</button>
    </div>
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
.timer-container {
  position: absolute;
  top: 40px;
  right: 80px;
  font-size: 20px;
  /* Add additional styles if needed */
}
.highlighted {
  background-color: red; /* Change the background color to highlight */
  padding: 2px 4px; /* Add padding to make it stand out more */
  border-radius: 4px; /* Add rounded corners */
}

.panel {
  position: absolute;
  top: 38px;
  left: 400px;
  width: 567px;
  height: 10px;
  flex-shrink: 0;
  background: #40bf9c;
}
.inner-progress-bar {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  background: var(--eden, #247158); /* Inner progress bar color */
}

.overall-progress-bar {
  position: absolute;
  top: 38px;
  left: 430px;
  width: 44px;
  height: 10px;
  flex-shrink: 0;
  background: var(--eden, #247158);
}

.language-Question-page .group-2 {
  position: absolute;
  top: 40px;
  right: 140px;
  width: 20px;
  height: 20px;
  flex-shrink: 0;
}
.language-Question-page .group-3 {
  position: absolute;
  top: 39px;
  left: 73px;
  width: 17px;
  height: 17px;
  flex-shrink: 0;
}

.language-Question-page .text-wrapper-11 {
  position: absolute;
  top: 38px;
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
  top: 107px;
  left: 405px;
  color: #2d3846;
  font-family: 'Fira Sans';
  font-size: 19px;
  font-style: normal;
  font-weight: 400;
  line-height: 37.5px; /* 150% */
  width: 632px;
}
.language-Question-page .text-wrapper-15 {
  position: absolute;
  width: 454px;
  left: 409px;
  top: 153px;
  color: #2d3846;
  font-family: 'Fira Sans';
  font-size: 16px;
  font-style: normal;
  font-weight: 400;
  line-height: 24px; /* 150% */
}
.language-Question-page .text-wrapper-16 {
  position: absolute;
  top: 268px;
  left: 405px;
  width: 632px;
  color: #000;
  font-family: 'Fira Sans';
  font-size: 18px;
  font-style: normal;
  font-weight: 400;
  line-height: 37.5px; /* 187.5% */
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
  top: 308px;
  left: 403px;
  width: 567px;
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
  font-size: 14px;
  font-style: normal;
  font-weight: 700;
  line-height: 16px; /* 114.286% */
}

.language-Question-page .div-wrapper {
  position: absolute;
  top: 668px;
  left: 1056px;
  width: 124px;
  height: 41px;
  flex-shrink: 0;
  border-radius: 10px;
  background: var(--new-mix-button-color, linear-gradient(218deg, #172554 -5.84%, #1e40af 106.73%));
  box-shadow: 0px 8px 21px 0px rgba(0, 0, 0, 0.16);
}
</style>
