<script setup>
import { ref, onMounted } from "vue";

import Test from "./components/Test.vue";

const isDarkMode = ref(true);

onMounted(() => {
  const savedTheme = localStorage.getItem("theme");

  if (savedTheme === "dark") {
    isDarkMode.value = true;
  }
});

const toggleDarkMode = () => {
  isDarkMode.value = !isDarkMode.value;

  localStorage.setItem("theme", isDarkMode.value ? "dark" : "light");
};
</script>

<template>
  <div>
    <button
      @click="toggleDarkMode"
      class="mt-6 bg-indigo-500 text-white px-4 py-2 shadow-lg"
    >
      {{ isDarkMode ? "Light Mode" : "Dark Mode" }}
    </button>
  </div>
  <main>
    <Test :isDarkMode="isDarkMode" />
  </main>
</template>

<style scoped></style>
