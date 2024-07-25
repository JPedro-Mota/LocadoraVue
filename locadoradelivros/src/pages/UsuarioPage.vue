<template>
  <div class="title">
    <h6>
      <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960" width="24px" fill="#000000"><path d="M40-160v-112q0-34 17.5-62.5T104-378q62-31 126-46.5T360-440q66 0 130 15.5T616-378q29 15 46.5 43.5T680-272v112H40Zm720 0v-120q0-44-24.5-84.5T666-434q51 6 96 20.5t84 35.5q36 20 55 44.5t19 53.5v120H760ZM360-480q-66 0-113-47t-47-113q0-66 47-113t113-47q66 0 113 47t47 113q0 66-47 113t-113 47Zm400-160q0 66-47 113t-113 47q-11 0-28-2.5t-28-5.5q27-32 41.5-71t14.5-81q0-42-14.5-81T544-792q14-5 28-6.5t28-1.5q66 0 113 47t47 113Z"/></svg>
      Usuário
    </h6>
  </div>
  <q-page padding>
    <TableComponents :columns="columns" :rows="rows" />
  </q-page>
</template>

<style>
.title {
  padding-left: 40px;
}
.actions-bt {
  background: none;
  border: none;
  cursor: pointer;
  padding: 5px;
}
</style>

<script setup>
import TableComponents from '../components/TableComponents.vue'

const columns = [
  { name: 'name', required: true, label: 'Nome do Livro', align: 'center', field: row => row.name, sortable: true },
  { name: 'editora', label: 'Editora', field: 'editora', sortable: true, align: 'center' },
  { name: 'autor', label: 'Autor', field: 'autor', sortable: true, align: 'center' },
  { name: 'data', label: 'Data', field: 'data', align: 'center' },
  { name: 'actions', label: 'Ações', align: 'center', field: 'actions', // Define the actions column
    format: row => `
      <q-btn flat icon="visibility" @click="viewBook('${row.name}')" class="actions-bt" />
      <q-btn flat icon="edit" @click="editBook('${row.name}')" class="actions-bt" />
      <q-btn flat icon="delete" @click="deleteBook('${row.name}')" class="actions-bt" />
    `
  }
]

const rows = [
  { name: 'Livro 1', editora: 'Editora A', autor: 'João da Silva', data: '12/12/2024', actions: '' },
  { name: 'Livro 2', editora: 'Editora B', autor: 'Maria de Souza', data: '01/01/2023', actions: '' },
  { name: 'Livro 3', editora: 'Editora C', autor: 'Pedro Pereira', data: '15/05/2022', actions: '' },
  { name: 'Livro 4', editora: 'Editora D', autor: 'Lucas Lima', data: '10/10/2021', actions: '' },
  { name: 'Livro 5', editora: 'Editora E', autor: 'Ana Silva', data: '20/09/2020', actions: '' },
  { name: 'Livro 6', editora: 'Editora F', autor: 'Carlos Alberto', data: '30/08/2019', actions: '' },
  { name: 'Livro 7', editora: 'Editora G', autor: 'Juliana Lima', data: '15/07/2018', actions: '' },
  { name: 'Livro 8', editora: 'Editora H', autor: 'Felipe Souza', data: '25/06/2017', actions: '' },
  { name: 'Livro 9', editora: 'Editora I', autor: 'Renata Santos', data: '05/05/2016', actions: '' }
]

// Define the methods for button actions
const viewBook = (name) => {
  console.log('Viewing book:', name)
}

const editBook = (name) => {
  console.log('Editing book:', name)
}

const deleteBook = (name) => {
  console.log('Deleting book:', name)
}
</script>
