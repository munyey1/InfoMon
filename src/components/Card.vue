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

const fetchPokemon = async () => {
  const genUrl = `https://pokeapi.co/api/v2/pokemon/1/`;

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

const testBtn = () => {
  console.log(types.value[0]);
  console.log(typeColorMap[types.value[0]]);
};
</script>

<template>
  <div :class="isDarkMode ? 'dark-mode' : ''">
    <div class="card bg-white dark:bg-slate-800 size-1/3 shadow-xl">
      <button @click="testBtn">Test</button>
      <figure>
        <div
          v-for="sprites in spriteUrl"
          :key="sprites"
          class="grid grid-rows-1 gap-4"
        >
          <img :src="sprites" class="h-auto w-fit p-4" />
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
