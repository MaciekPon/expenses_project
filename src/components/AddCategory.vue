<template>
  <q-dialog ref="dialogRef" @hide="onDialogHide">
    <q-card class="q-dialog-plugin">
      <q-input type="text" label="Nowa kategoria" v-model="newCategory" />
      <q-card-actions align="right">
        <q-btn color="primary" label="OK" @click="onOKClick" />
        <q-btn color="primary" label="Cancel" @click="onDialogCancel" />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<script setup>
import { useDialogPluginComponent } from "quasar";
import { ref } from "vue";

const newCategory = ref("");

const props = defineProps({
  alert: Boolean,
});

defineEmits([...useDialogPluginComponent.emits]);

const { dialogRef, onDialogHide, onDialogOK, onDialogCancel } =
  useDialogPluginComponent();

// this is part of our example (so not required)
function onOKClick() {
  // on OK, it is REQUIRED to
  // call onDialogOK (with optional payload)
  onDialogOK({ newCategory: newCategory.value });
  // or with payload: onDialogOK({ ... })
  // ...and it will also hide the dialog automatically
}
</script>
<style></style>
