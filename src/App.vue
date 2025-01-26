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
    <div
      class="dark:bg-slate-900 bg-white h-screen max-h-screen"
    >   
      <div role="tablist" class="tabs tabs-bordered">
        <input
          type="radio"
          name="my_tabs_1"
          role="tab"
          class="tab text-slate-900 dark:text-slate-400 whitespace-nowrap ml-10"
          aria-label="Search"
          checked="checked"
        />
        <div role="tabpanel" class="tab-content px-10 py-6">
          <Main :isDarkMode="isDarkMode" />
        </div>

        <input
          type="radio"
          name="my_tabs_1"
          role="tab"
          class="tab text-slate-900 dark:text-slate-400 whitespace-nowrap"
          aria-label="Compare"
        />
        <div role="tabpanel" class="tab-content p-10">
        </div>

        <button
          @click="toggleDarkMode"
          class="btn mt-6 bg-indigo-500 text-white mx-4 shadow-lg whitespace-nowrap"
        >
          {{ isDarkMode ? "Light Mode" : "Dark Mode" }}
        </button>
      </div>
    </div>
  </main>
</template>

<style scoped></style>
