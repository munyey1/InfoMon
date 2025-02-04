<script setup>
import { ref, onMounted, computed } from "vue";
import axios, { all } from "axios";

import Card from "./components/Card.vue";
import Info from "./components/Info.vue";

const isDarkMode = ref(true);

const allPokemon = ref([]);
const loading = ref(true);
const error = ref(null);

const pokemonData = ref([]);

const selectedPokemonUrl = ref("");
const searchQuery = ref("");

const fetchPokemon = async () => {
  const genUrl = `https://pokeapi.co/api/v2/pokemon?limit=100000&offset=0/`;

  try {
    const response = await axios.get(genUrl);
    allPokemon.value = response.data.results;

    const batchSize = 200;
    const allPokemonDetails = [];

    for(let i = 0; i < allPokemon.value.length; i += batchSize) {
      const batch = allPokemon.value.slice(i, i + batchSize);
      const batchDetails = await Promise.all(
        batch.map((pokemon) => axios.get(pokemon.url).then((res) => res.data))
      );

      allPokemonDetails.push(...batchDetails);
    }
    
    pokemonData.value = allPokemonDetails;

  } catch (err) {
    error.value = err;
  } finally {
    loading.value = false;
  }
};

const toggleDarkMode = () => {
  isDarkMode.value = !isDarkMode.value;

  localStorage.setItem("theme", isDarkMode.value ? "dark" : "light");
};

const selPokemon = (url) => {
  selectedPokemonUrl.value = url;
};

const filteredPokemon = computed(() => {
  return allPokemon.value.filter((pokemon) =>
    pokemon.name.toLowerCase().includes(searchQuery.value.toLowerCase())
  );
});

onMounted(() => {
  fetchPokemon();
  const savedTheme = localStorage.getItem("theme");

  if (savedTheme === "dark") {
    isDarkMode.value = true;
  }
});

const tstBtn = () => {
  console.log(allPokemon, pokemonData.value);
  for(let i = 0; i < 10; i++) {
    console.log(allPokemon.value[i]);
  }
} 

</script>

<template>
  <main :class="isDarkMode ? 'dark-mode' : ''" class="h-screen flex flex-col">
    <div class="dark:bg-slate-800 bg-white min-h-screen">
      <div
        class="bg-white dark:bg-slate-800 px-6 shadow-xl grid grid-cols-5 flex items-stretch"
      >
        <div class="col-span-3 pt-6 flex flex-col">
          <button
            @click="toggleDarkMode"
            class="btn bg-indigo-500 text-white mx-4 shadow-lg whitespace-nowrap w-32"
          >
            {{ isDarkMode ? "Light Mode" : "Dark Mode" }}
          </button>
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
        <div class="col-span-2 flex flex-col pt-4 h-screen sticky top-0">
          <div>
            <div>
              <input
                v-model="searchQuery"
                type="text"
                class="input dark:bg-slate-900 bg-white text-slate-900 dark:text-slate-400 shadow-lg"
                placeholder="Search Pokemon"
              />
              <button @click="tstBtn" class="btn bg-indigo-500 text-white">
                test
              </button>
            </div>
          </div>
          <div class="mt-4 flex-grow overflow-y-auto h-screen p-2">
            <div class="grid grid-cols-3 gap-2">
              <Card
                v-memo="[pokemon.url]"
                @click="selPokemon(pokemon.url)"
                v-for="pokemon in filteredPokemon"
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
  </main>
</template>

<style scoped></style>
