<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
import { defineProps } from "vue";

const props = defineProps({
  isDarkMode: Boolean,
  url: String,
});

const spriteUrl = ref("");
const loading = ref(true);
const error = ref(null);

const fetchPokemon = async () => {
  const genUrl = `https://pokeapi.co/api/v2/pokemon/1/`;

  try {
    const response = await axios.get(genUrl);
    spriteUrl.value = response.data.sprites.front_default;
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
        <img
          :src=spriteUrl
          alt="Shoes"
        />
      </figure>
      <div class="card-body">
        <h2 class="card-title">
          Shoes!
          <div class="badge badge-secondary">NEW</div>
        </h2>
        <p>If a dog chews shoes whose shoes does he choose?</p>
        <div class="card-actions justify-end">
          <div class="badge badge-outline">Fashion</div>
          <div class="badge badge-outline">Products</div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
