<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";

const allPokemon = ref([]);
const loading = ref(true);
const error = ref(null);

const selectedGen = ref("");

const fetchPokemon = async () => {
  const url = "https://pokeapi.co/api/v2/pokemon?limit=151";
  // Figure out how to get the selected generation
  // const url = `https://pokeapi.co/api/v2/generation/${selectedGen.value}`;
  // Seems like this is the correct API call

  try {
    const response = await axios.get(url);
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

const get151Pokemon = () => {
  // Use selectedGen.value to get the correct generation
  fetchPokemon();
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
        <select class="select bg-white dark:bg-slate-800 w-full max-w-xs" v-model="selectedGen">
          <option value="" selected disabled>Please select</option>
          <option value="1">Generation 1</option>
          <option value="2">Generation 2</option>
          <option value="3">Generation 3</option>
          <option value="4">Generation 4</option>
          <option value="5">Generation 5</option>
          <option value="6">Generation 6</option>
          <option value="7">Generation 7</option>
          <option value="8">Generation 8</option>
        </select>
      </div>
      <button
        @click="getPokemon"
        class="btn mt-6 bg-indigo-500 text-white px-4 py-2 shadow-lg"
      >
        Get 151 Pokemon
      </button>
      <div v-if="error" class="mt-4">
        <p class="text-slate-500 dark:text-slate-400 text-sm">
          {{ error.message }}
        </p>
      </div>
      <div v-else class="mt-4">
        <ul>
          <li v-for="pokemon in allPokemon" :key="pokemon.name">
            {{ pokemon.name }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
