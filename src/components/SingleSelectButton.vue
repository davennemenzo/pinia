<template>
  <div>
    <!-- Display the question text -->
    <p>{{ currentQuestionText }}</p>

    <!-- Loop through options and create clickable boxes -->
    <div
      v-for="(option, index) in currentOptions"
      :key="index"
      class="option-box p-4 mb-2 border rounded-xs cursor-pointer bg-blue-400 mx-auto justify-center"
      style="width: 320px"
      :class="{ 'bg-blue-100': isSelected(option) }"
      @click="handleOptionSelection(option)"
    >
      <!-- Display the image if it exists -->
      <img
        v-if="option.image"
        :src="option.image"
        alt="Option image"
        class="w-full mb-2 rounded"
      />

      <!-- Display the text if it exists -->
      <p v-if="option.text">{{ option.text }}</p>

      <!-- Hidden radio input -->
      <input
        type="radio"
        :id="`option-${index}`"
        :name="`question-${currentQuestionId}`"
        :value="option"
        v-model="selectedOption"
        class="hidden"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

// Array of questions with options that can have both text and images
const questions = [
  {
    id: 1,
    text: 'What is your favorite color?',
    options: [
      { text: 'Red', image: 'https://i.pinimg.com/236x/93/fa/35/93fa35b5a5d30810a1b653eec30c6283.jpg' },
      { text: 'Blue', image: 'https://i.pinimg.com/564x/fc/63/a5/fc63a565a1655ca9b9eddc330e9a3986.jpg' },
      { text: 'Green', image: 'https://i.pinimg.com/236x/51/7e/f8/517ef8e8b2a3ab78489188231b38915e.jpg' },
      { text: 'Yellow', image: 'https://i.pinimg.com/236x/51/7e/f8/517ef8e8b2a3ab78489188231b38915e.jpg' },
    ],
  },
  {
    id: 2,
    text: 'What is your favorite animal?',
    options: [
      { text: 'Dog', image: 'pat/dog-image.jpgh/to' },
      { text: 'Cat', image: 'path/to/cat-image.jpg' },
      { text: 'Bird', image: null },
      { text: 'Fish', image: 'path/to/fish-image.jpg' },
    ],
  },
  // Add more questions as needed
]

// Track the index of the current question
const currentQuestionIndex = ref(0)

// Computed properties for current question data
const currentQuestion = computed(() => questions[currentQuestionIndex.value])
const currentQuestionId = computed(() => currentQuestion.value.id)
const currentQuestionText = computed(() => currentQuestion.value.text)
const currentOptions = computed(() => currentQuestion.value.options)

// Reactive reference for the selected option
const selectedOption = ref('')

// Check if the option is selected
function isSelected(option) {
  return selectedOption.value === option
}

// Handle option selection
function handleOptionSelection(option) {
  selectedOption.value = option
  emitSelectedOption()
  moveToNextQuestion()
}

// Emit the selected option and question ID
function emitSelectedOption() {
  emit('answered', currentQuestionId.value, selectedOption.value)
}

// Move to the next question
function moveToNextQuestion() {
  if (currentQuestionIndex.value < questions.length - 1) {
    currentQuestionIndex.value++
    selectedOption.value = ''
  } else {
    console.log('No more questions')
  }
}

// Define emit function to handle custom events
const emit = defineEmits(['answered'])
</script>

<style scoped>
.option-box {
  display: flex;
  align-items: center;
  flex-direction: column;
  text-align: center;
}

.hidden {
  display: none;
}

img {
  max-width: 100%;
  height: auto;
  border-radius: 8px;
}
</style>
