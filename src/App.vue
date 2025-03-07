<script setup lang="ts">
import { computed, onMounted, ref } from "vue";

interface Exercise {
  name: string;
  calories: number;
  duration: number;
}

const exercises = ref<Exercise[]>([]);
const newExercise = ref<Exercise>({
  name: "",
  calories: 0,
  duration: 0,
});

const totalCalories = computed(() => {
  return exercises.value.reduce((total, exercise) => {
    return total + exercise.calories * (exercise.duration / 60);
  }, 0);
});

function saveExercises() {
  localStorage.setItem("exercises", JSON.stringify(exercises.value));
}

function loadExercises() {
  const savedExercises = localStorage.getItem("exercises");
  if (savedExercises) {
    exercises.value = JSON.parse(savedExercises);
  }
}

function addExercise() {
  if (
    newExercise.value.name &&
    newExercise.value.calories > 0 &&
    newExercise.value.duration > 0
  ) {
    exercises.value.push({ ...newExercise.value });
    newExercise.value = { name: "", calories: 0, duration: 0 };
    saveExercises();
  }
}

function removeExercise(index: number) {
  exercises.value.splice(index, 1);
  saveExercises();
}

onMounted(() => {
  loadExercises();
});
</script>

<template>
  <div class="container mx-auto px-4 py-10 md:px-6 lg:px-12">
    <h1
      class="text-4xl md:text-5xl lg:text-6xl font-bold text-center mb-10 text-white"
    >
      Workout Calculator
    </h1>
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      <div
        v-for="(exercise, index) in exercises"
        :key="exercise.name"
        class="bg-white p-6 rounded-lg shadow-md md:p-8 lg:p-10 relative"
      >
        <h2
          class="text-2xl md:text-3xl lg:text-4xl font-bold mb-4 text-purple-500"
        >
          {{ exercise.name }}
        </h2>
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
          <input
            type="number"
            id="duration"
            class="mt-1 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md"
            v-model="exercise.duration"
          />
        </p>
        <button
          @click="removeExercise(index)"
          class="absolute top-2 right-2 text-red-500 hover:text-red-700"
        >
          &times;
        </button>
      </div>
    </div>
    <div class="my-8 text-center">
      <h2 class="text-2xl md:text-3xl lg:text-4xl font-bold mb-6 text-white">
        New Exercise
      </h2>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <label class="block text-2xl font-medium text-white"
          >Exercise Name</label
        >
        <label class="block text-2xl font-medium text-white"
          >Burned Calories</label
        >
        <label class="block text-2xl font-medium text-white"
          >Exercise Duration</label
        >
      </div>
    </div>
    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
      <input
        type="text"
        placeholder="Exercise Name"
        v-model="newExercise.name"
        class="p-2 rounded-md border-gray-300"
      />
      <input
        type="number"
        placeholder="Calories"
        v-model="newExercise.calories"
        class="p-2 rounded-md border-gray-300"
      />
      <input
        type="number"
        placeholder="Duration (minutes)"
        v-model="newExercise.duration"
        class="p-2 rounded-md border-gray-300"
      />
    </div>
    <div class="mt-8 mb-6 flex justify-center">
      <button
        @click="addExercise"
        class="px-4 py-2 font-bold rounded-md transition duration-300 text-purple-500 bg-white hover:bg-purple-600 hover:text-white"
      >
        Add Exercise
      </button>
    </div>
  </div>
  <div class="text-center">
    <button
      type="button"
      class="px-4 py-2 font-bold text-purple-500 bg-white rounded-md hover:bg-purple-600 hover:text-white"
    >
      Calculate
    </button>
    <h2 class="text-2xl md:text-3xl lg:text-4xl font-bold mt-6 text-white">
      Total calories burned: {{ totalCalories }}
    </h2>
  </div>
</template>
