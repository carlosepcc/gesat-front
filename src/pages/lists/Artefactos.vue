<template>
  <q-page class="q-pb-xl">
    <ArtefactoForm v-model="showForm" @close-form="closeForm" />
    <q-btn size="sm" dense flat icon="refresh" @click="listarArtefactos" />

    <ListPage
      @open-form="(payload) => openForm(payload)"
      @delete-rows="(selectedRows) => deleteTuples(selectedRows)"
      rowKey="id"
      heading="Artefactos"
      :rows="artefactosArr"
      :columns="artefactoFields"
    />
  </q-page>
</template>

<script setup>
import ListPage from 'src/components/ListPage.vue';
import ArtefactoForm from 'components/forms/ArtefactoForm';
import { provide, ref } from 'vue';
import listar from 'src/composables/useAPI'
import { eliminar } from 'src/composables/useAPI'

const artefactoFields = ref([
  { name: 'nombre', required: true, label: 'Nombre', align: 'left', field: 'nombre', sortable: true, },
  { name: 'descripcion', align: 'left', label: 'Descripción', field: 'descripcion', sortable: true, },
  { name: 'fase', label: 'Fase', field: 'fase', sortable: true },
  { name: 'disciplina', label: 'Disciplina', field: 'disciplina' },
]);

const artefactosArr = ref([])
provide('artefactosArr', artefactosArr)
const url = '/artefacto'
provide('artefactoUrl', url)

//listar
const listarArtefactos = () => listar(artefactosArr, url)
provide('listarArtefactos', listarArtefactos)
// execute on component load
listarArtefactos()


//form dialog model
const showForm = ref(false);

//closeForm triggered on: Cancel
const closeForm = () => {
  showForm.value = false
}

// MODIFICAR (Abrir formulario con datos del objeto a modificar)
const artefactoObject = ref()
provide('artefactoObject', artefactoObject)

//openForm triggered on: Nueva entrada, Modificar
const openForm = (obj = { nombre: `Artefacto ${artefactosArr.value.length + 1}`, fase: 'Inicio', disciplina: 'Modelado del negocio' }) => {
  artefactoObject.value = obj
  showForm.value = true
}

// delete tuples by array of objects
const deleteTuples = (selectedRows = []) => eliminar(selectedRows, artefactosArr, url)

</script>
