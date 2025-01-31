<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";

import Card from "./Card.vue";
import Info from "./Info.vue";

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
  selectedPokemonUrl.value = url;
};
</script>

<template>
  <div :class="isDarkMode ? 'dark-mode' : ''" class="h-screen flex flex-col">
    <div
      class="bg-white dark:bg-slate-800 px-6 py-8 shadow-xl grid grid-cols-5 flex items-stretch"
    >
      <div class="col-span-3 p-2 flex flex-col">
        <div class="flex flex-col items-center">
          <div
            v-if="selectedPokemonUrl"
            class="capitalize text-slate-900 dark:text-slate-400 text-sm mt-4 w-full"
          >
            <Info
              :url="selectedPokemonUrl"
              :key="selectedPokemonUrl"
              :isDarkMode="isDarkMode"
            />
          </div>
          <p
            v-else
            class="text-slate-900 dark:text-slate-400 text-2xl font-bold mt-4"
          >
            Select a Pokemon to view more information
          </p>
        </div>
      </div>
      <div class="border-2 col-span-2 p-2 flex flex-col h-full">
        <div>
          <div>
            <input
              v-model="selectedGenQuery"
              type="text"
              class="input mb-4 dark:bg-slate-900 bg-white text-slate-900 dark:text-slate-400 shadow-lg"
              placeholder="Search Pokemon"
            />
          </div>
          <div>
            <button
              class="btn bg-indigo-500 text-white px-4 py-2 mr-2 shadow-lg"
            >
              Search "Dummy"
            </button>
            <button
              @click="resetPokemon"
              class="btn bg-indigo-500 text-white px-4 py-2 mx-2 shadow-lg"
            >
              Empty
            </button>
          </div>
        </div>
        <div class="border-2 mt-4 flex-grow overflow-y-auto h-screen p-2">
          <div class="grid grid-cols-3 gap-2">
            <Card
              @click="testBtn(pokemon.url)"
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
