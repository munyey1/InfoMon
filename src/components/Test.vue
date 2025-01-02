<script setup>
import { ref, onMounted } from "vue";

const isDarkMode = ref(false);

const toggleDarkMode = () => {
  isDarkMode.value = !isDarkMode.value;

  localStorage.setItem("theme", isDarkMode.value ? "dark" : "light");
};

onMounted(() => {
  const savedTheme = localStorage.getItem("theme");

  if (savedTheme === "dark") {
    isDarkMode.value = true;
  }
});
</script>

<template>
  <div :class="isDarkMode ? 'dark-mode' : ''">
    <div
      class="bg-white dark:bg-slate-800 rounded-lg px-6 py-8 ring-1 ring-slate-900/5 shadow-xl"
    >
      <div>
        <span
          class="inline-flex items-center justify-center p-2 bg-indigo-500 rounded-md shadow-lg"
        >
          <svg
            class="h-6 w-6 text-white"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            aria-hidden="true"
          >
            <!-- ... -->
          </svg>
        </span>
      </div>
      <h3
        class="text-slate-900 dark:text-white mt-5 text-base font-medium tracking-tight"
      >
        Writes Upside-Down
      </h3>
      <p class="text-slate-500 dark:text-slate-400 mt-2 text-sm">
        The Zero Gravity Pen can be used to write in any orientation, including
        upside-down. It even works in outer space.
      </p>
      <button
        @click="toggleDarkMode"
        class="mt-6 bg-indigo-500 text-white px-4 py-2 rounded-md shadow-lg"
      >
        {{ isDarkMode ? "Light Mode" : "Dark Mode" }}
      </button>
    </div>
  </div>
</template>

<style scoped></style>
