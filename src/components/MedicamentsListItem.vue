<script setup>
import { ref } from 'vue';
const props = defineProps(['med']);
defineEmits(['eventDelete', 'eventIncrement', 'eventDecrement', 'eventEdit']);

const isEditing = ref(false);

const handleFileUpload = (event) => {
  const file = event.target.files[0];
  if (!file) return; // Si l'utilisateur annule la sélection du fichier, on ne fait rien
  // FileReader est un objet JavaScript permettant de lire le contenu d'un fichier
  // de manière asynchrone.
  const reader = new FileReader();
  reader.onload = () => {
    // definir le traitement asynchrone du contenu du fichier
    props.med.photo = reader.result; // --> convertit le contenu du fichier en base64
  };
  reader.readAsDataURL(file); // lance la lecture du fichier et donc la conversion en base64
};
</script>

<template>
  <li>
    <div v-if="!isEditing">
      {{ med.denomination }} - Stock: {{ med.qte }} - Forme:
      {{ med.formepharmaceutique }}
      <img
        v-if="med.photo"
        :src="'https://apipharmacie.pecatte.fr/images/' + med.photo"
        alt="image du medicament"
        width="150"
      />
      <button @click="$emit('eventIncrement', med)">+1</button>
      <button @click="$emit('eventDecrement', med)">-1</button>
      <button @click="isEditing = true">Modifier</button>
      <button @click="$emit('eventDelete', med.id)">Supprimer</button>
    </div>

    <div v-else>
      <input v-model="med.denomination" type="text" />
      <input v-model="med.qte" type="number" />
      <input v-model="med.formepharmaceutique" type="text" />
      <input id="photo" @change="handleFileUpload" type="file" />
      <button
        @click="
          $emit('eventEdit', med);
          isEditing = false;
        "
      >
        Valider
      </button>
      <button @click="isEditing = false">Annuler</button>
    </div>
  </li>
</template>
