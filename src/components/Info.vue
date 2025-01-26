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

const fetchPokemon = async () => {
  try {
    const response = await axios.get(props.url);
    pokemon.value = response.data;
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
  <div v-if="error">
    {{ error.message }}
  </div>
  <div v-if="pokemon">
    {{ pokemon.name }}
  </div>
</template>

<style scoped></style>
