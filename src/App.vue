<script setup>
import { ref, onMounted } from "vue";

import Main from "./components/Main.vue";

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
  <main :class="isDarkMode ? 'dark-mode' : ''">
    <div class="dark:bg-slate-800 bg-white">
      <button
        @click="toggleDarkMode"
        class="btn mt-6 bg-indigo-500 text-white mx-4 shadow-lg whitespace-nowrap"
      >
        {{ isDarkMode ? "Light Mode" : "Dark Mode" }}
      </button>
      <Main :isDarkMode="isDarkMode" />
    </div>
  </main>
</template>

<style scoped></style>
