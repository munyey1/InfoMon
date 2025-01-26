<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";

import { typeColorMap } from "@/utils/typeColourMap";

import ToolTip from "./ToolTip.vue";

const props = defineProps({
  isDarkMode: Boolean,
  url: String,
});

const pokemon = ref(null);
const loading = ref(true);
const error = ref(null);

const sprites = ref([]);
const imgUrl = ref("");
const showShiny = ref(false);

const types = ref([]);

const fetchPokemon = async () => {
  try {
    const response = await axios.get(props.url);

    pokemon.value = response.data;
    sprites.value = response.data.sprites.other["official-artwork"];
    imgUrl.value =
      response.data.sprites.other["official-artwork"].front_default;
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
</script>

<template>
  <div :class="isDarkMode ? 'dark-mode' : ''" class="w-full">
    <div v-if="error">
      {{ error.message }}
    </div>
    <div
      v-if="pokemon"
      class="grid grid-cols-2 justify-items-center content-center"
    >
      <h1 class="text-2xl font-bold col-span-2 mb-4">
        {{ pokemon.name }}
      </h1>
      <div class="col-span-2">
        <div
          v-for="type in types"
          :key="type"
          class="badge badge-lg text-white capitalize font-semibold mr-2"
          :class="typeColorMap[type] || 'bg-gray-500'"
        >
          {{ type }}
        </div>
      </div>
      <figure class="col-span-2">
        <img v-if="showShiny" :src="sprites.front_shiny" />
        <img v-else :src="sprites.front_default" />
      </figure>
      <button
        @click="showShiny = !showShiny"
        class="col-span-2 btn bg-indigo-500 text-white -mt- mb-10"
      >
        {{ showShiny ? "Show Default" : "Show Shiny" }}
      </button>
      <div class="space-y-1">
        <div class="text-base space-y-1">
          <p>
            Pokedex No: <span class="font-bold">#{{ pokemon.id }}</span>
          </p>
          <p>
            Height: <span class="font-bold">{{ pokemon.height / 10 }}m</span>
          </p>
          <p>
            Weight: <span class="font-bold">{{ pokemon.weight / 10 }}kg</span>
          </p>
        </div>
      </div>
      <div>
        <div class="text-base">
          <p class="font-bold">Abilities</p>
          <ul>
            <li
              v-for="ability in pokemon.abilities"
              :key="ability.ability.name"
            >
              <ToolTip :url="ability.ability.url" />
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
