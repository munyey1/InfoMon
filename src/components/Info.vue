<script setup>
import { ref, computed } from "vue";

import { typeColorMap } from "@/utils/typeColourMap";

import ToolTip from "./ToolTip.vue";

const props = defineProps({
  isDarkMode: Boolean,
  pokemon: Object,
});

const showShiny = ref(false);

const sprites = computed(() => {
  if (!props.pokemon) return [];
  return Object.values(props.pokemon.sprites).filter(
    (sprite) => typeof sprite === "string"
  ).filter(Boolean);
});
</script>

<template>
  <div v-if="props.pokemon" :class="isDarkMode ? 'dark-mode' : ''" class="w-full pb-6">
    <div class="grid grid-cols-2 justify-items-center content-center">
      <h1 class="text-4xl font-bold col-span-2 mb-4">
        {{ props.pokemon?.name || "Unknown Pokémon" }}
      </h1>
      <div class="col-span-2">
        <div
          v-for="type in props.pokemon.types"
          :key="type.type.name"
          class="badge badge-lg text-white capitalize font-semibold mr-2"
          :class="typeColorMap[type.type.name] || 'bg-gray-500'"
        >
          {{ type.type.name }}
        </div>
      </div>
      <figure class="col-span-2">
        <img
          v-if="showShiny"
          :src="props.pokemon.sprites.other['official-artwork'].front_shiny"
        />
        <img
          v-else
          :src="props.pokemon.sprites.other['official-artwork'].front_default"
        />
      </figure>
      <button
        @click="showShiny = !showShiny"
        class="col-span-2 btn bg-indigo-500 text-white -mt- mb-10"
      >
        {{ showShiny ? "Show Default" : "Show Shiny" }}
      </button>
      <div>
        <div class="font-semibold text-base space-y-1">
          <p>
            Pokedex No: <span class="font-bold">#{{ props.pokemon.id }}</span>
          </p>
          <p>
            Height: <span class="font-bold">{{ props.pokemon.height / 10 }}m</span>
          </p>
          <p>
            Weight: <span class="font-bold">{{ props.pokemon.weight / 10 }}kg</span>
          </p>
        </div>
        <div class="mt-10">
        <p class="text-2xl font-bold">Sprites:</p>
        <div class="grid grid-cols-2 gap-2">
          <img
            v-for="sprite in sprites"
            :key="sprite"
            :src="sprite"
            class="h-auto w-full"
          />
        </div>
        <p v-if="sprites.length === 0" class="text-lg text-slate-900 dark:text-slate-400">
          No Sprites Available
        </p>
      </div>
      </div>
      <div>
        <div>
          <p class="text-2xl font-bold">Abilities</p>
          <ul>
            <li
              v-for="ability in props.pokemon.abilities"
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
