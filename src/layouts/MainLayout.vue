<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-toolbar-title> Quasar App </q-toolbar-title>
        <span class="q-mr-sm row align-center">Wydałeś: <strong>{{ expensesCount }}</strong></span>

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

onMounted(async () => {
  await axios.get(`${import.meta.env.VITE_LOCAL}api/expenses`).then((response) => {
    response.data.map(
      (item) => (expensesCount.value += parseInt(item.expense))
    );
  });
});
</script>
