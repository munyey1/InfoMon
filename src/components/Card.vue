<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
import { defineProps } from "vue";

import { typeColorMap } from "@/utils/typeColourMap";

const props = defineProps({
  isDarkMode: Boolean,
  name: String,
  url: String,
});

const loading = ref(true);
const error = ref(null);

const pokemon = ref(null);
const spriteUrl = ref([]);
const types = ref([]);
const showShiny = ref(false);

const fetchPokemon = async () => {
  const genUrl = props.url;

  try {
    const response = await axios.get(genUrl);
    pokemon.value = response.data;
    spriteUrl.value = response.data.sprites.other["official-artwork"];

    types.value = pokemon.value.types.map((type) => type.type.name);
  } catch (err) {
    error.value = err;
  } finally {
    loading.value = false;
  }
};

onMounted(() => {
  fetchPokemon();
});

const toggleShiny = () => {
  showShiny.value = !showShiny.value;
};

const testBtn = () => {
  console.log(spriteUrl.value.front_default);
};
</script>

<template>
  <div :class="isDarkMode ? 'dark-mode' : ''">
    <div class="card bg-white dark:bg-slate-800 shadow-xl">
      <figure>
        <div class="grid grid-cols-1 justify-items-center">
          <button
            @click="toggleShiny"
            class="btn bg-indigo-500 text-white mt-8"
          >
            Toggle Sprite
          </button>
          <p class="mt-4 text-lg font-medium">
            {{ showShiny ? "Shiny" : "Default" }}
          </p>
          <img
            v-if="showShiny"
            :src="spriteUrl.front_shiny"
            class="h-auto w-full p-4"
          />
          <img
            v-else
            :src="spriteUrl.front_default"
            class="h-auto w-full p-4"
          />
        </div>
      </figure>
      <div class="card-body">
        <h2 class="card-title capitalize">
          {{ props.name }}
          <div
            v-for="type in types"
            :key="type"
            class="badge text-white capitalize"
            :class="typeColorMap[type] || 'bg-gray-500'"
          >
            {{ type }}
          </div>
        </h2>
        
      </div>
    </div>
  </div>
</template>

<style scoped></style>
