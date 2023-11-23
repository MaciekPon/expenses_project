<template>
  <q-page>
    <div style="display: flex; flex-direction: column">
      <q-input type="number" label="Wpisz kwote" v-model="expense" />
    </div>

    <div class="q-mt-md">
      <q-select
        v-model="category"
        :options="categories"
        option-label="name"
        option-value="name"
        emit-value
        label="Wybierz kategorie"
      />
    </div>

    <div class="q-mt-md">
      <q-btn class="q-mr-md" @click="addToExpenses"> Dodaj wpis </q-btn>

      <q-btn @click="addCategory">Dodaj kategorie</q-btn>
    </div>

    <q-list class="q-mt-md" bordered separator>
      <q-item class="row justify-between" v-for="(expense, index) in expenses" :key="index">
        {{ expense.category }} {{ expense.expense }}
        <q-btn @click="deleteExpense(expense.id)">Usuń</q-btn>
      </q-item>
    </q-list>
  </q-page>
</template>

<script setup>
import axios from "axios";
import { ref, onMounted, computed } from "vue";
import { useQuasar } from "quasar";
import AddCategory from "src/components/AddCategory.vue";
const API_URL = "http://localhost:5038/";

const $q = useQuasar();
const expenses = ref([]);
const expense = ref(0);
const category = ref("");
const showAlert = ref(false);
const categories = ref([]);

const addCategory = () => {
  showAlert.value = true;
  $q.dialog({
    component: AddCategory,
    componentProps: {
      alert: showAlert.value,
    },
  })
    .onOk((payload) => {
      axios
        .post(`${API_URL}api/new-category`, { name: payload.newCategory })
        .then((response) => {
          refreshData(true);
        });
      // console.log('OK')
    })
    .onCancel(() => {
      // console.log('Cancel')
    })
    .onDismiss(() => {
      // console.log('I am triggered on both OK and Cancel')
    });
};

async function refreshData(category) {
  if (!category) {
    await axios.get(API_URL + "api/expenses").then((response) => {
      expenses.value = response.data;
    });
  } else {
    await axios.get(API_URL + "api/categories").then((response) => {
      categories.value = response.data;
    });
  }
}

async function addToExpenses() {
  const result = {
    expense: expense.value,
    category: category.value,
  };

  await axios.post(`${API_URL}api/add-expense`, result).then((response) => {
    refreshData();
    alert(response.data);
  });
  expense.value = 0;
  category.value = "";
}

async function deleteExpense(id) {
  await axios.delete(`${API_URL}api/delete?id=${id}`).then((response) => {
    refreshData();
    alert(response.data);
  });
}

onMounted(async () => {
  refreshData();
  refreshData(true);
});
</script>
