<script setup>
import { ref } from 'vue';
const libelle = ref(''); 
const emit = defineEmits(['eventSearch']); 
</script>

<template>
  <v-card elevation="3" rounded="lg" class="mb-6">
    <v-card-title class="text-primary font-weight-bold">
      <v-icon icon="mdi-magnify" class="mr-2" />
      Rechercher
    </v-card-title>

    <v-card-text>
      <v-text-field
        v-model="libelle"
        label="Nom du médicament"
        variant="outlined"
        density="compact"
        prepend-inner-icon="mdi-magnify"
        hide-details
        clearable
        @keyup.enter="$emit('eventSearch', libelle)"
        @click:clear="$emit('eventSearch', '')"
      />
    </v-card-text>

    <v-card-actions>
      <v-btn
        color="primary"
        variant="flat"
        prepend-icon="mdi-magnify"
        block
        @click="$emit('eventSearch', libelle)"
      >
        Rechercher
      </v-btn>
    </v-card-actions>
  </v-card>
</template>
