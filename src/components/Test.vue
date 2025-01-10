<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";

const allPokemon = ref([]);
const loading = ref(true);
const error = ref(null);

const selectedGenQuery = ref("");

const fetchPokemon = async () => {
  const genUrl = `https://pokeapi.co/api/v2/generation/${selectedGenQuery.value}/`;

  try {
    const response = await axios.get(genUrl);
    allPokemon.value = response.data.results;
  } catch (err) {
    error.value = err;
  } finally {
    loading.value = false;
  }
};

const props = defineProps({
  isDarkMode: Boolean,
});

const getPokemon = () => {
  fetchPokemon();
  console.log(allPokemon.value);
};
</script>

<template>
  <div :class="isDarkMode ? 'dark-mode' : ''">
    <div
      class="bg-white dark:bg-slate-800 px-6 py-8 ring-1 ring-slate-900/5 shadow-xl"
    >
      <h3
        class="text-slate-900 dark:text-white mt-5 text-base font-medium tracking-tight"
      >
        Pokemon
      </h3>
      <div>
        <select
          class="select bg-white dark:bg-slate-800 w-full max-w-xs"
          v-model="selectedGenQuery"
        >
          <option selected disabled>Please select</option>
          <option value="limit=151">Generation 1</option>
          <option value="limit=100&offset=151">Generation 2</option>
          <option value="limit=135&offset=251">Generation 3</option>
          <option value="limit=107&offset=386">Generation 4</option>
          <option value="limit=156&offset=493">Generation 5</option>
          <option value="limit=72&offset=649">Generation 6</option>
          <option value="limit=88&offset=721">Generation 7</option>
          <option value="limit=96&offset=809">Generation 8</option>
        </select>
      </div>
      <button
        @click="getPokemon"
        class="btn mt-6 bg-indigo-500 text-white px-4 py-2 shadow-lg"
      >
        Search Pokemon
      </button>
      <div v-if="error" class="mt-4">
        <p class="text-slate-500 dark:text-slate-400 text-sm">
          {{ error.message }}
        </p>
      </div>
      <div v-else class="mt-4">
        <ul>
          <li v-for="pokemon in allPokemon" :key="pokemon.name">
            <p class="text-slate-900 dark:text-white">{{ pokemon.name }}</p>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
