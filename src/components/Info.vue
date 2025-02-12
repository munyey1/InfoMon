<script setup>
import { ref, computed } from "vue";

import { typeColorMap } from "@/utils/typeColourMap";

import ToolTip from "./ToolTip.vue";
import Moves from "./Moves.vue";

const props = defineProps({
  isDarkMode: Boolean,
  pokemon: Object,
});

const showShiny = ref(false);
const selSprite = ref(null);

const sprites = computed(() => {
  if (!props.pokemon) return [];
  return Object.values(props.pokemon.sprites)
    .filter((sprite) => typeof sprite === "string")
    .filter(Boolean);
});

const stats = computed(() => {
  if (!props.pokemon) return [];
  return props.pokemon.stats.map((stat) => ({
    name: stat.stat.name,
    base_stat: stat.base_stat,
  }));
});

const showModal = (sprite) => {
  selSprite.value = sprite;
  const modal = document.getElementById("img_modal");
  modal.showModal();
};

const maxStat = (name, stat) => {
  if (name === "hp") {
    return Math.floor(stat * 2 + 204);
  } else {
    return Math.floor((stat * 2 + 99) * 1.1);
  }
};

const minStat = (name, stat) => {
  if (name === "hp") {
    return Math.floor(stat * 2 + 110);
  } else {
    return Math.floor((stat * 2 + 5) * 0.9);
  }
};

const tstbtn = () => {
  console.log("test");
  console.log(props.pokemon.game_indices);
  console.log(props.pokemon.moves);
};
</script>

<template>
  <div
    v-if="props.pokemon"
    :class="isDarkMode ? 'dark-mode' : ''"
    class="w-full pb-6"
  >
    <!--Top Half-->
    <div class="grid grid-cols-3 justify-items-center content-center">
      <h1 class="text-4xl font-bold col-span-3 mb-4">
        {{ props.pokemon?.name || "Unknown Pokémon" }}
      </h1>
      <div class="col-span-3">
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
      <!--Sprites-->
      <div class="flex flex-col justify-center items-center col-span-1">
        <p class="text-2xl font-bold">Sprites:</p>
        <div class="grid grid-cols-2 gap-2">
          <button v-for="sprite in sprites" @click="showModal(sprite)">
            <img :key="sprite" :src="sprite" class="h-auto w-full" />
          </button>
        </div>
        <p
          v-if="sprites.length === 0"
          class="text-lg text-slate-900 dark:text-slate-400"
        >
          No Sprites Available
        </p>
        <dialog id="img_modal" class="modal">
          <div class="modal-box dark:bg-slate-900 bg-white p-4">
            <img :src="selSprite" class="h-auto w-full" alt="Pokemon Sprite" />
          </div>
          <form method="dialog" class="modal-backdrop">
            <button>close</button>
          </form>
        </dialog>
      </div>
      <button
        @click="showShiny = !showShiny"
        class="col-span-3 btn bg-indigo-500 text-white -mt- mb-10"
      >
        {{ showShiny ? "Show Default" : "Show Shiny" }}
      </button>
      <button @click="tstbtn" class="col-span-3 btn bg-indigo-500 text-white">
        test
      </button>
    </div>
    <!--Bottom Half-->
    <div class="grid grid-cols-2 justify-items-center content-start">
      <p class="col-span-2 text-2xl font-bold mb-10">PokeDex Data</p>
      <!--Basic Info-->
      <div>
        <div class="font-semibold text-base space-y-1">
          <p>
            Pokedex No: <span class="font-bold">#{{ props.pokemon.id }}</span>
          </p>
          <p>
            Height:
            <span class="font-bold">{{ props.pokemon.height / 10 }}m</span>
          </p>
          <p>
            Weight:
            <span class="font-bold">{{ props.pokemon.weight / 10 }}kg</span>
          </p>
        </div>
        <!--Stats-->
        <div class="mt-10">
          <p class="text-xl font-bold">Base Stats</p>
          <ul class="text-base space-y-1">
            <li class="mb-2" v-for="stat in stats" :key="stat.name">
              {{ stat.name }}:
              <span class="font-bold">{{ stat.base_stat }}</span>
              <br />
              <progress
                class="progress progress-primary w-56"
                :value="stat.base_stat"
                :max="maxStat(stat.name, stat.base_stat) / 2"
              ></progress>
              Min:
              <span class="font-bold">{{
                minStat(stat.name, stat.base_stat)
              }}</span>
              Max:
              <span class="font-bold">{{
                maxStat(stat.name, stat.base_stat)
              }}</span>
            </li>
          </ul>
        </div>
      </div>
      <!--Abilities-->
      <div>
        <p class="text-xl font-bold">Abilities</p>
        <ul>
          <li
            v-for="ability in props.pokemon.abilities"
            :key="ability.ability.name"
          >
            <ToolTip :url="ability.ability.url" />
          </li>
        </ul>
      </div>
      <p class="text-2xl col-span-2 font-bold mt-10">Moveset</p>
      <div class="col-span-2">
        <Moves :isDarkMode="isDarkMode" :moves="props.pokemon.moves" />
      </div>
    </div>
  </div>
</template>

<style scoped></style>
