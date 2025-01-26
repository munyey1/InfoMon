<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";

const props = defineProps({
  isDarkMode: Boolean,
  url: String,
});

const pokemon = ref(null);
const loading = ref(true);
const error = ref(null);

const imgUrl = ref("");

const fetchPokemon = async () => {
  try {
    const response = await axios.get(props.url);
    pokemon.value = response.data;
    imgUrl.value = response.data.sprites.other["official-artwork"].front_default;
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
  <div :class="isDarkMode ? 'dark-mode' : ''" class="border-2 w-full">
    <div v-if="error">
      {{ error.message }}
    </div>
    <div v-if="pokemon" class="grid grid-cols-2 justify-items-center">
      <h1 class="text-2xl font-bold col-span-2">
        {{ pokemon.name }}
      </h1>
      <figure class="col-span-2">
        <img :src="imgUrl" />
      </figure>
      <div class="text-base">
        <p>
          Pokedex No: <span class="font-bold">#{{ pokemon.id }}</span>
        </p>
        <p>
          Height: <span class="font-bold">{{ pokemon.height/10 }}m</span>
        </p>
        <p>
          Weight: <span class="font-bold">{{ pokemon.weight/10 }}kg</span>
        </p>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
