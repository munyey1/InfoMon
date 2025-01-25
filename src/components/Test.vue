<script setup>
import { ref } from "vue";
import axios from "axios";

import Card from "./Card.vue";

const props = defineProps({
  isDarkMode: Boolean,
});

const allPokemon = ref([]);
const loading = ref(true);
const error = ref(null);

const selectedGenQuery = ref("");

const fetchPokemon = async () => {
  const genUrl = `https://pokeapi.co/api/v2/pokemon?${selectedGenQuery.value}/`;

  try {
    const response = await axios.get(genUrl);
    allPokemon.value = response.data.results;
  } catch (err) {
    error.value = err;
  } finally {
    loading.value = false;
  }
};

const getPokemon = () => {
  fetchPokemon();
};

const emptyPokemon = () => {
  allPokemon.value = [];
  error.value = null;
  loading.value = true;
};
</script>

<template>
  <div :class="isDarkMode ? 'dark-mode' : ''">
    <div
      class="bg-white dark:bg-slate-800 px-6 py-8 ring-1 ring-slate-900/5 shadow-xl min-h-screen"
    >
      <div class="flex flex-col justify-center items-center">
        <h3
          class="text-slate-900 dark:text-white text-2xl font-semibold mb-4"
        >
          Search Pokemon - Prototype 1
        </h3>
        <div class="mb-4">
          <select
            class="select bg-white text-slate-900 dark:text-slate-400 dark:bg-slate-800 w-full max-w-xs"
            v-model="selectedGenQuery"
          >
            <option value=""selected disabled>Select Generation</option>
            <option value="limit=151&offset=0">Generation 1</option>
            <option value="limit=100&offset=151">Generation 2</option>
            <option value="limit=135&offset=251">Generation 3</option>
            <option value="limit=107&offset=386">Generation 4</option>
            <option value="limit=156&offset=493">Generation 5</option>
            <option value="limit=72&offset=649">Generation 6</option>
            <option value="limit=88&offset=721">Generation 7</option>
            <option value="limit=96&offset=809">Generation 8</option>
          </select>
        </div>
        <div class="">
          <button
            @click="getPokemon"
            class="btn bg-indigo-500 text-white px-4 py-2 mx-2 shadow-lg"
          >
            Search
          </button>
          <button
            @click="emptyPokemon"
            class="btn bg-indigo-500 text-white px-4 py-2 mx-2 shadow-lg"
          >
            Empty
          </button>
        </div>
      </div>
      <div v-if="error" class="mt-4">
        <p class="text-slate-500 dark:text-slate-400 text-sm">
          {{ error.message }}
        </p>
      </div>
      <div v-else class="mt-4">
        <div class="grid grid-cols-3 gap-4">
          <Card
            v-for="pokemon in allPokemon"
            :key="pokemon.name"
            :isDarkMode="isDarkMode"
            :name="pokemon.name"
            :url="pokemon.url"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
