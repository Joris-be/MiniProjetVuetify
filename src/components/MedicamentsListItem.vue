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
  <div v-if="!isEditing">
    <v-card elevation="3" rounded="lg">
      <v-img
        v-if="med.photo"
        :src="'https://apipharmacie.pecatte.fr/images/' + med.photo"
        alt="image du medicament"
        height="175"
        cover
      />
      <v-card-title class="text-primary font-weight-bold text-center">
        {{ med.denomination }}
      </v-card-title>
      <v-card-text class="text-center">
        <v-chip color="green" variant="elevated" size="default"
          >Stock : {{ med.qte }}
        </v-chip>
      </v-card-text>
      <v-card-subtitle
        class="text-primary text-center font-italic font-weight-bold"
        >{{ med.formepharmaceutique }}</v-card-subtitle
      >
      <v-card-actions class="flex-wrap ga-4 pa-3">
        <v-btn
          icon="mdi-plus"
          color="green"
          variant="elevated"
          size="small"
          @click="$emit('eventIncrement', med)"
        />
        <v-btn
          icon="mdi-minus"
          color="warning"
          variant="elevated"
          size="small"
          @click="$emit('eventDecrement', med)"
        />
        <v-btn
          icon="mdi-pencil"
          color="primary"
          variant="elevated"
          size="small"
          @click="isEditing = true"
        />
        <v-btn
          icon="mdi-delete"
          color="red"
          variant="elevated"
          size="small"
          @click="$emit('eventDelete', med.id)"
        />
      </v-card-actions>
    </v-card>
  </div>

  <div v-else>
    <v-card elevation="3" rounded="lg">
      <v-card-title class="text-primary font-weight-bold">
        <v-icon icon="mdi-pencil-circle" class="mr-2" />
        Modification
      </v-card-title>

      <v-card-text class="d-flex flex-column ga-3">
        <v-text-field
          v-model="med.denomination"
          label="Nom du médicament"
          variant="outlined"
          density="compact"
          prepend-inner-icon="mdi-pill"
          hide-details
        />
        <v-text-field
          v-model="med.formepharmaceutique"
          label="Forme pharmaceutique"
          variant="outlined"
          density="compact"
          prepend-inner-icon="mdi-shape"
          hide-details
        />
        <v-text-field
          v-model.number="med.qte"
          label="Quantité"
          type="number"
          variant="outlined"
          density="compact"
          prepend-inner-icon="mdi-package-variant"
          hide-details
          :min="0"
        />
        <v-file-input
          label="Photo"
          variant="outlined"
          density="compact"
          hide-details
          @change="handleFileUpload"
        />
      </v-card-text>

      <v-card-actions class="d-flex ga-2">
        <v-btn
          color="primary"
          variant="flat"
          prepend-icon="mdi-check"
          flex
          @click="
            $emit('eventEdit', med);
            isEditing = false;
          "
        >
          Valider
        </v-btn>
        <v-btn
          color="error"
          variant="outlined"
          prepend-icon="mdi-close"
          flex
          @click="isEditing = false"
        >
          Annuler
        </v-btn>
      </v-card-actions>
    </v-card>
  </div>
</template>
