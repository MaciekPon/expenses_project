<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-toolbar-title> Quasar App </q-toolbar-title>
        <p class="q-mr-sm">Wydałeś {{ expensesCount }}</p>

        <div>Quasar v{{ $q.version }}</div>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
const expensesCount = ref(0);
const API_URL = "http://localhost:5038/";

onMounted(async () => {
  await axios.get(`${API_URL}api/expenses`).then((response) => {
    response.data.map(
      (item) => (expensesCount.value += parseInt(item.expense))
    );
  });
});
</script>
