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
  reader.onload = () => { // definir le traitement asynchrone du contenu du fichier
  photo.value = reader.result // --> convertit le contenu du fichier en base64
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
  <form @submit.prevent="submitForm">
    <input
      v-model="denomination"
      type="text"
      placeholder="Nom du médicament"
      required
    />
    <input
      v-model="formepharmaceutique"
      type="text"
      placeholder="Forme pharmaceutique"
      required
    />
    <input
      v-model.number="qte"
      type="number"
      placeholder="Quantité"
      min="0"
      required
    />
    <input id="photo" @change="handleFileUpload" type="file" />

    <button type="submit">Ajouter</button>
  </form>
</template>
