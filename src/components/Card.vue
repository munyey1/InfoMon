<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
import { defineProps } from "vue";

import { typeColorMap } from "@/utils/typeColourMap";

import ToolTip from "./ToolTip.vue";

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
    spriteUrl.value = response.data.sprites;

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
  console.log(pokemon.value.height, pokemon.value.weight);
  console.log(pokemon.value.abilities);
};
</script>

<template>
  <div :class="isDarkMode ? 'dark-mode' : ''">
    <div class="card bg-white dark:bg-slate-800 shadow-xl">
      <div class="card-body">
        <h2 class="card-title text-slate-900 dark:text-slate-400 capitalize">
          {{ props.name }}
        </h2>
        <figure>
          <div class="">
            <img
              v-if="showShiny"
              :src="spriteUrl.front_shiny"
              class="h-auto w-fit"
            />
            <img
              v-else
              :src="spriteUrl.front_default"
              class="h-auto w-full"
            />
          </div>
        </figure>
        <div
          v-for="type in types"
          :key="type"
          class="badge text-white capitalize font-semibold"
          :class="typeColorMap[type] || 'bg-gray-500'"
        >
          {{ type }}
        </div>
        <div v-if="!loading" class="text-base font-semibold text-slate-900 dark:text-slate-400">
            #{{ pokemon.id }}
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
