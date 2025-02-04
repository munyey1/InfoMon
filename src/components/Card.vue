<script setup>
import { defineProps } from "vue";

import { typeColorMap } from "@/utils/typeColourMap";

const props = defineProps({
  isDarkMode: Boolean,
  pokemon: Object,
});

</script>

<template>
  <div :class="isDarkMode ? 'dark-mode' : ''">
    <div class="card bg-white dark:bg-slate-800 shadow-xl cursor-pointer">
      <div class="card-body">
        <h2 class="card-title text-slate-900 dark:text-slate-400 capitalize">
          {{ props.pokemon.name }}
        </h2>
        <figure>
          <img
            :src="props.pokemon.sprites.front_default"
            loading="lazy"
            class="h-auto w-full"
          />
        </figure>
        <div class="">
          <div
            v-for="type in props.pokemon.types"
            :key="type.type.name"
            class="badge text-white capitalize font-semibold mr-2"
            :class="typeColorMap[type.type.name] || 'bg-gray-500'"
          >
            {{ type.type.name }}
          </div>
        </div>
        <div class="text-base font-semibold text-slate-900 dark:text-slate-400">
            #{{ props.pokemon.id }}
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
