<script setup>
import MedicamentsList from './components/MedicamentsList.vue';
</script>

<template>
  <v-app>
    <v-app-bar color="primary" elevation="4">
      <v-app-bar-title>
        <v-icon icon="mdi-pill" class="mr-2" />
        Pharmacie - Gestion des médicaments
      </v-app-bar-title>
    </v-app-bar>

    <v-main class="bg-grey-lighten-4">
      <v-container class="py-8">
        <MedicamentsList />
      </v-container>
    </v-main>
  </v-app>
</template>
