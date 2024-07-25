<template>
  <q-table
    :rows="rows"
    :columns="columns"
    row-key="name"
  >
    <template v-slot:body-cell-actions="props">
      <q-td :props="props">
        <slot name="actions" v-bind:row="props.row"></slot>
      </q-td>
    </template>
  </q-table>
</template>

<script>
export default {
  name: 'TableComponents',
  props: {
    rows: Array,
    columns: Array,
  },
}
</script>
