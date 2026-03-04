<script setup>
import { ref } from 'vue';
const denomination = ref(''); // le nom que l’utilisateur tape
const formepharmaceutique = ref('Comprimé'); // valeur par défaut
const qte = ref(1); // quantité par défaut
const photo = ref(null); // --> variable qui va contenir le fichier en base64

const emit = defineEmits(['eventAdd']);

const handleFileUpload = (event) => {
  const file = event.target.files[0];
  if (!file) return; // Si l'utilisateur annule la sélection du fichier, on ne fait rien
  // FileReader est un objet JavaScript permettant de lire le contenu d'un fichier
  // de manière asynchrone.
  const reader = new FileReader();
  reader.onload = () => {
    // definir le traitement asynchrone du contenu du fichier
    photo.value = reader.result; // --> convertit le contenu du fichier en base64
  };
  reader.readAsDataURL(file); // lance la lecture du fichier et donc la conversion en base64
};

function submitForm() {
  // Crée un objet médicament complet
  const med = {
    denomination: denomination.value,
    formepharmaceutique: formepharmaceutique.value,
    qte: qte.value,
    photo: photo.value,
  };

  emit('eventAdd', med); // envoie l’objet au parent
  denomination.value = '';
  formepharmaceutique.value = '';
  qte.value = 1;
  photo.value = null;
}
</script>

<template>
  <v-card elevation="3" rounded="lg">
    <v-card-title class="text-primary font-weight-bold">
      <v-icon icon="mdi-plus-circle" class="mr-2" />
      Ajouter un médicament
    </v-card-title>

    <v-card-text class="d-flex flex-column ga-3">
      <v-text-field
        v-model="denomination"
        label="Nom du médicament"
        variant="outlined"
        density="compact"
        prepend-inner-icon="mdi-pill"
        hide-details
        required
      />
      <v-text-field
        v-model="formepharmaceutique"
        label="Forme pharmaceutique"
        variant="outlined"
        density="compact"
        prepend-inner-icon="mdi-shape"
        hide-details
        required
      />
      <v-text-field
        v-model.number="qte"
        label="Quantité"
        type="number"
        variant="outlined"
        density="compact"
        prepend-inner-icon="mdi-package-variant"
        hide-details
        :min="0"
        required
      />
      <v-file-input
        label="Photo"
        variant="outlined"
        density="compact"
        hide-details
        @change="handleFileUpload"
      />
    </v-card-text>

    <v-card-actions>
      <v-btn
        color="primary"
        variant="flat"
        prepend-icon="mdi-plus"
        block
        @click="submitForm"
      >
        Ajouter
      </v-btn>
    </v-card-actions>
  </v-card>
</template>
