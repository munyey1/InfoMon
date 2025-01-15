<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
import { defineProps } from "vue";

const props = defineProps({
  isDarkMode: Boolean,
  url: String,
});

const pokemon = ref(null);
const spriteUrl = ref([]);
const loading = ref(true);
const error = ref(null);

const types = ref([]);

const tt = ref("green-500");

const fetchPokemon = async () => {
  const genUrl = `https://pokeapi.co/api/v2/pokemon/1/`;

  try {
    const response = await axios.get(genUrl);
    pokemon.value = response.data;
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

const testBtn = () => {
  types.value = pokemon.value.types.map((type) => type.type.name);
  console.log(types.value[0]);
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
          <img :src="sprites" class="h-auto w-fit p-4"/>
        </div>
      </figure>
      <div class="card-body">
        <h2 class="card-title">Bulbasaur
          <div class="badge text-white" :class="`bg-${tt}`">Grass</div>
          <div class="badge bg-fuchsia-500 text-white">Poison</div>
        </h2> 
        <p>Check out Bulbasaur's official artwork!</p>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
