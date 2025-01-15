<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
import { defineProps } from "vue";

const props = defineProps({
  isDarkMode: Boolean,
  url: String,
});

const spriteUrl = ref([]);
const loading = ref(true);
const error = ref(null);

const fetchPokemon = async () => {
  const genUrl = `https://pokeapi.co/api/v2/pokemon/1/`;

  try {
    const response = await axios.get(genUrl);
    spriteUrl.value = response.data.sprites.other["official-artwork"];
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
  <div :class="isDarkMode ? 'dark-mode' : ''">
    <div class="card bg-white dark:bg-slate-800 shadow-xl">
      <figure>
        <div
          v-for="sprites in spriteUrl"
          :key="sprites"
          class="grid grid-rows-1 gap-4 justify-center items-center"
        >
          <img :src="sprites" class="h-auto w-1/2 border-2 mx-auto"/>
        </div>
      </figure>
      <div class="card-body">
        <h2 class="card-title">Bulbasaur</h2> 
        <p>Check out Bulbasaur's official artwork!</p>
        <div class="card-actions justify-end">
          <div class="badge badge-outline">Grass</div>
          <div class="badge badge-outline">Poison</div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
