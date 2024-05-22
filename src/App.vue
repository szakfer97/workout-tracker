<script setup lang="ts">
import { ref, watch } from 'vue'


interface Exercise {
  name: string
  calories: number
  duration: number
}

const exercises = ref<Exercise[]>([
  {
    name: 'Walking',
    calories: 150,
    duration: 30,
  },
  {
    name: 'Jogging',
    calories: 350,
    duration: 30,
  },
  {
    name: 'Running',
    calories: 500,
    duration: 30,
  },
  {
    name: 'Swimming',
    calories: 200,
    duration: 30,
  },
])

const totalCalories = ref(0)

function calculateTotalCalories() {
  totalCalories.value = exercises.value.reduce((total, exercise) => total + exercise.calories * (exercise.duration / 60), 0)
}

watch(exercises, calculateTotalCalories)
</script>

<template>
  <div class="container mx-auto px-4 py-10 md:px-6 lg:px-12">
    <h1 class="text-4xl md:text-5xl lg:text-6xl font-bold text-center mb-10">Workout Calculator</h1>
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      <div v-for="exercise in exercises" :key="exercise.name" class="bg-white p-6 rounded-lg shadow-md md:p-8 lg:p-10">
        <h2 class="text-2xl md:text-3xl lg:text-4xl font-bold mb-4">{{ exercise.name }}</h2>
        <p>
          <span class="font-bold">Calories:</span>
          {{ exercise.calories }}
        </p>
        <p>
          <span class="font-bold">Duration:</span>
          {{ exercise.duration }} minutes
        </p>
        <p class="mt-6">
          <label for="duration" class="block text-sm font-medium text-gray-700">
            Duration (minutes)
          </label>
          <input type="number" id="duration" class="mt-1 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md"
            v-model="exercise.duration" />
        </p>
      </div>
    </div>
    <div class="mt-10 text-center">
      <button type="button" class="px-4 py-2 font-bold text-white bg-blue-600 rounded-md hover:bg-blue-700"
        @click="calculateTotalCalories">
        Calculate
      </button>
      <h2 class="text-2xl md:text-3xl lg:text-4xl font-bold mt-6">
        Total calories burned: {{ totalCalories }}
      </h2>
    </div>
  </div>
</template>
