<script setup>
import { reactive, onMounted, ref } from 'vue';
import MedicamentsForm from './MedicamentsForm.vue';
import MedicamentsListItem from './MedicamentsListItem.vue';
import MedicamentsSearch from './MedicamentsSearch.vue';

const critere = ref('');
const listeMedicaments = reactive([]);
const url = 'https://apipharmacie.pecatte.fr/api/22/medicaments';

function getMedicaments() {
  listeMedicaments.length = 0;
  fetch(`${url}?search=${critere.value}`, { method: 'GET' })
    .then((response) => response.json())
    .then((dataJSON) => {
      dataJSON.forEach((med) => listeMedicaments.push(med));
    })
    .catch((error) => {
      console.log(error);
    });
}
onMounted(() => {
  getMedicaments();
});

function handlerAddMedicament(med) {
  let myHeaders = new Headers();
  myHeaders.append('Content-Type', 'application/json');
  const fetchOptions = {
    method: 'POST',
    headers: myHeaders,
    body: JSON.stringify(med), // ici on envoie l'objet JSON
  };
  fetch(url, fetchOptions)
    .then((response) => response.json())
    .then((dataJSON) => {
      listeMedicaments.push(dataJSON);
    })
    .catch((error) => {
      console.log(error);
    });
}

function handlerDeleteMedicament(id) {
  fetch(`${url}/${id}`, {
    method: 'DELETE',
  })
    .then(() => {
      getMedicaments();
    })
    .catch((error) => {
      console.log(error);
    });
}

function handlerIncrementMedicament(med) {
  const medModifie = {
    id: med.id, // obligatoire
    qte: med.qte + 1, // champ à modifier
  };
  fetch(url, {
    method: 'PUT',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(medModifie),
  })
    .then(() => {
      getMedicaments(); // recharge la liste
    })
    .catch((error) => console.log(error));
}

function handlerDecrementMedicament(med) {
  const medModifie = {
    id: med.id,
    qte: med.qte - 1,
  };
  fetch(url, {
    method: 'PUT',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(medModifie),
  })
    .then(() => {
      getMedicaments();
    })
    .catch((error) => console.log(error));
}

function AddHandler(search) {
  critere.value = search;
  getMedicaments();
}

function handlerEditMedicament(med) {
  fetch(url, {
    method: 'PUT',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(med),
  })
    .then(() => getMedicaments())
    .catch((error) => console.log(error));
}
</script>

<template>
  <v-row>
    <v-col cols="12" md="4">
      <MedicamentsSearch @eventSearch="handlerSearchMedicament" />
      <MedicamentsForm @eventAdd="handlerAddMedicament" />
    </v-col>

    <v-col cols="12" md="8">
      <h2 class="text-h5 font-weight-bold text-primary mb-4">
        <v-icon icon="mdi-pill" class="mr-2" />
        Liste des médicaments
      </h2>
      <v-row>
        <v-col
          v-for="med in listeMedicaments"
          :key="med.id"
          cols="12"
          sm="6"
          lg="4"
        >
          <MedicamentsListItem
            :med="med"
            @eventDelete="handlerDeleteMedicament"
            @eventIncrement="handlerIncrementMedicament"
            @eventDecrement="handlerDecrementMedicament"
            @eventEdit="handlerEditMedicament"
          />
        </v-col>
      </v-row>
    </v-col>
  </v-row>
</template>
