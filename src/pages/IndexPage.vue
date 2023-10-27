<template>
  <AddCategory v-bind="{ alert: showAlert }" />

  <q-page>
    <div style="display: flex; flex-direction: column">
      <q-input type="number" label="Wpisz kwote" v-model="expense" />
    </div>

    <div class="q-mt-md">
      <q-select
        v-model="category"
        :options="options"
        label="Wybierz kategorie"
      />
    </div>

    <div class="q-mt-md">
      <q-btn class="q-mr-md" @click="addToExpenses(expense, category)">
        Dodaj wpis
      </q-btn>

      <q-btn @click="addCategory">Dodaj kategorie</q-btn>
    </div>

    <q-list class="q-mt-md" bordered separator>
      <q-item v-for="(expense, index) in expenses" :key="index">
        {{ expense.category }} {{ expense.value }}
      </q-item>
    </q-list>
  </q-page>
</template>

<script setup>
import { ref } from "vue";
import { useQuasar } from "quasar";
import AddCategory from "src/components/AddCategory.vue";

const $q = useQuasar();
const expenses = ref([]);
const expense = ref("");
const category = ref("");
const showAlert = ref(false);
const options = ref(["Dom", "Inne"]);

const addCategory = () => {
  showAlert.value = true;
  $q.dialog({
    component: AddCategory,
    componentProps: {
      alert: showAlert.value,
    },
  })
    .onOk((payload) => {
      options.value.push(payload.newCategory);
      console.log(payload);
      // console.log('OK')
    })
    .onCancel(() => {
      // console.log('Cancel')
    })
    .onDismiss(() => {
      // console.log('I am triggered on both OK and Cancel')
    });
};

const addToExpenses = (sendedExpense, sendedCategory) => {
  expenses.value.push({
    value: sendedExpense,
    category: sendedCategory,
  });
};
</script>
