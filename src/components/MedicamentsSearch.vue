<script setup>
import { ref } from 'vue';
const libelle = ref(''); // c’est une variable réactive
const emit = defineEmits(['eventSearch']); // définir l'événement eventAdd
</script>

<template>
  <v-card color="black" rounded="lg" hover>
    <v-card-title class="text-primary font-weight-bold">
      <v-icon icon="mdi-magnify" />
      Recherche de Médicament :
    </v-card-title>

    <form @submit.prevent="$emit('eventSearch', libelle)">
      <input v-model="libelle" type="text" placeholder="Nom de Médicament" />
      <button type="submit">Rechercher</button>
    </form>
  </v-card>
</template>
