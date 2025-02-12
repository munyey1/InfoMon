<script setup>
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
        <h2 class="card-title font-bold text-slate-900 dark:text-slate-400 capitalize">
          {{ props.pokemon.name }}
        </h2>
        <figure>
          <img
            v-if="props.pokemon.sprites.front_default"
            :src="props.pokemon.sprites.front_default"
            class="h-auto w-full"
          />
          <p v-else class="text-slate-900 dark:text-slate-400 text-center">
            No Sprite Available
          </p>
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
