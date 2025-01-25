<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";

import Card from "./Card.vue";

const props = defineProps({
  isDarkMode: Boolean,
});

const allPokemon = ref([]);
const loading = ref(true);
const error = ref(null);

const selectedPokemonUrl = ref("");

const selectedGenQuery = ref("");

const fetchPokemon = async () => {
  const genUrl = `https://pokeapi.co/api/v2/pokemon?limit=100000&offset=0/`;

  try {
    const response = await axios.get(genUrl);
    allPokemon.value = response.data.results;
  } catch (err) {
    error.value = err;
  } finally {
    loading.value = false;
  }
};

onMounted(() => {
  fetchPokemon();
});

const resetPokemon = () => {
  fetchPokemon();
};

const testBtn = (url) => {
  console.log(url);
  selectedPokemonUrl.value = url;
};

</script>

<template>
  <div :class="isDarkMode ? 'dark-mode' : ''">
    <div
      class="bg-white dark:bg-slate-800 px-6 py-8 ring ring-slate-900/5 shadow-xl min-h-screen grid grid-cols-5"
    >
      <div class="col-span-3">
        <div class="flex flex-col justify-center items-center">
          <h3
            class="text-slate-900 dark:text-white text-2xl font-semibold mb-4"
          >
            Search Pokemon - Prototype 2
          </h3>
          <div>
          </div>
          <div class="capitalize text-slate-900 dark:text-slate-400 text-sm mt-4">
            {{ selectedPokemonUrl }}
          </div>
        </div>
      </div>
      <div class="col-span-2">
        <button
          class="btn bg-indigo-500 text-white px-4 py-2 mx-2 shadow-lg"
        >
          Search "Dummy"
        </button>
        <button
          @click="resetPokemon"
          class="btn bg-indigo-500 text-white px-4 py-2 mx-2 shadow-lg"
        >
          Empty
        </button>
        <div v-if="error" class="mt-4">
          <p class="text-slate-500 dark:text-slate-400 text-sm">
            {{ error.message }}
          </p>
        </div>
        <div v-else class="mt-4 overflow-y-auto max-h-screen p-2">
          <div class="grid grid-cols-3 gap-2">
            <Card
              @click="testBtn(pokemon.name)"
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
  </div>
</template>

<style scoped></style>
