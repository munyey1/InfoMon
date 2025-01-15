<script setup>
import { ref, onMounted } from "vue";

import Test from "./components/Test.vue";
import Search from "./components/Search.vue";
import Card from "./components/Card.vue";

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
      class="dark:bg-slate-900 bg-white min-h-screen flex items-center justify-center"
    >
      <div role="tablist" class="tabs tabs-bordered">
        <input
          type="radio"
          name="my_tabs_1"
          role="tab"
          class="tab"
          aria-label="Tab 1"
          checked="checked"
        />
        <div role="tabpanel" class="tab-content p-10">
          <Test :isDarkMode="isDarkMode" />
        </div>

        <input
          type="radio"
          name="my_tabs_1"
          role="tab"
          class="tab"
          aria-label="Tab 2"
        />
        <div role="tabpanel" class="tab-content p-10">
          <Card :isDarkMode="isDarkMode" :name="bulbasaur" />
        </div>

        <button
          @click="toggleDarkMode"
          class="btn mt-6 bg-indigo-500 text-white px-4 py-2 shadow-lg"
        >
          {{ isDarkMode ? "Light Mode" : "Dark Mode" }}
        </button>
      </div>
    </div>
  </main>
</template>

<style scoped></style>
