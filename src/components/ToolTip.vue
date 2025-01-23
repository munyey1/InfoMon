<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
import { defineProps } from "vue";

const props = defineProps({
  url: String,
});

const loading = ref(true);
const error = ref(null);

const ability = ref(null);
const abilityEntry = ref(null);

const fetchAbility = async () => {
  const abilityUrl = props.url;

  try {
    const response = await axios.get(abilityUrl);
    ability.value = response.data;
    abilityEntry.value = ability.value.effect_entries.find(
      (entry) => entry.language.name === "en"
    );
    
  } catch (err) {
    error.value = err;
  } finally {
    loading.value = false;
  }
};

onMounted(() => {
  fetchAbility();
});

</script>

<template>
  <div>
    <div v-if="loading">Loading...</div>
    <div v-else-if="error">Error: {{ error.message }}</div>
    <div v-if="abilityEntry" class="tooltip tooltip-right" :data-tip="abilityEntry.effect">
      <h1 class="cursor-pointer capitalize font-semibold">{{ ability.name }}</h1>
    </div>
  </div>
</template>

<style scoped>
</style>