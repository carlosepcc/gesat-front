<template>
  <q-page padding class="q-gutter-sm">
    <!-- TABLE / GRID -->
    <q-btn-toggle
      title="Modo de presentación (Tabla o Rejilla)"
      v-model="state.grid"
      push
      toggle-color="primary"
      :options="[
        { label: 'Vista en Tabla', noCaps: true, value: false, slot: 'tableview' },
        { label: 'Vista por Tarjetas', noCaps: true, value: true, slot: 'gridview' },
      ]"
    >
      <template v-slot:tableview>
        <q-icon name="r_table_chart" class="on-right" />
      </template>

      <template v-slot:gridview>
        <q-icon name="r_grid_view" class="on-right" />
      </template>
    </q-btn-toggle>
    <br />
    <!-- DENSE / NORMAL -->
    <q-btn-toggle
      title="Densidad las filas en vista de tabla (Normal o Denso)"
      v-model="state.dense"
      push
      class="q-mx-sm"
      toggle-color="primary"
      :options="[
        { label: 'Interfaz normal', noCaps: true, value: false, slot: 'normal' },
        { label: 'Interfaz densa', noCaps: true, value: true, slot: 'dense' },
      ]"
    >
      <template v-slot:normal>
        <q-icon name="r_table_rows" class="on-right" />
      </template>

      <template v-slot:dense>
        <q-icon name="view_headline" class="on-right" />
      </template>
    </q-btn-toggle>

    <q-input
      autofocus
      style="max-width: 20em"
      list="serverUrls"
      v-model="axiosBaseURL"
      placeholder="baseURL.."
      @change="setBaseURL"
    />
    <datalist id="serverUrls">
      <option value="http://mdc-gesat.herokuapp.com"></option>
      <option value="http://gesatserver.herokuapp.com"></option>
      <option value="http://localhost:9090"></option>
      <option value="http://10.8.44.191:9090"></option>
    </datalist>
  </q-page>
</template>

<script setup>
import { ref } from "vue"
import state from 'src/composables/useState'
import { api } from "boot/axios";
const axiosBaseURL = ref(api.defaults.baseURL)

const setBaseURL = (url = axiosBaseURL.value) => {
  api.defaults.baseURL = url
  console.log(api.defaults.baseURL)
}
</script>
