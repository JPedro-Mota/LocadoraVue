<template>
  <div class="title">
    <h6>
      <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960" width="24px" fill="#000000">
        <path d="M480-60q-72-68-165-104t-195-36v-440q101 0 194 36.5T480-498q73-69 166-105.5T840-640v440q-103 0-195.5 36T480-60Zm0-540q-66 0-113-47t-47-113q0-66 47-113t113-47q66 0 113 47t47 113q0 66-47 113t-113 47Z"/>
      </svg>
      Biblioteca
    </h6>
  </div>
  <q-page padding>
    <TableComponents :columns="columns" :rows="rows">
      <template #actions="{ row }">
        <div class="dialogsa">
          <q-btn flat round dense icon="visibility" @click="openViewDialog(row)" class="actions-bt" />
          <q-btn flat round dense icon="edit" @click="openEditDialog(row)" class="actions-bt" />
          <q-btn flat round dense icon="delete" @click="openDeleteDialog(row)" class="actions-bt" />

          <q-dialog v-model="viewDialog.visible" persistent>
            <q-card>
              <q-card-section>
                <div class="text-h6">Detalhes do Livro</div>
              </q-card-section>

              <q-card-section class="q-pt-none">
                <div><strong>Nome:</strong> {{ viewDialog.data.name }}</div>
                <div><strong>Editora:</strong> {{ viewDialog.data.editora }}</div>
                <div><strong>Autor:</strong> {{ viewDialog.data.autor }}</div>
                <div><strong>Data:</strong> {{ viewDialog.data.data }}</div>
              </q-card-section>

              <q-card-actions align="right">
                <q-btn flat label="Fechar" color="primary" v-close-popup />
              </q-card-actions>
            </q-card>
          </q-dialog>

          <q-dialog v-model="editDialog.visible" persistent>
            <q-card>
              <q-card-section>
                <div class="text-h6">Editar Livro</div>
              </q-card-section>

              <q-card-section class="q-pt-none">
                <!-- Formulário de edição do livro -->
                <q-input v-model="editDialog.data.name" label="Nome do Livro" />
                <q-input v-model="editDialog.data.editora" label="Editora" />
                <q-input v-model="editDialog.data.autor" label="Autor" />
                <q-input v-model="editDialog.data.data" label="Data" />
              </q-card-section>

              <q-card-actions align="right">
                <q-btn flat label="Salvar" color="primary" @click="saveEdit" />
                <q-btn flat label="Cancelar" color="primary" v-close-popup />
              </q-card-actions>
            </q-card>
          </q-dialog>

          <q-dialog v-model="deleteDialog.visible" persistent>
            <q-card>
              <q-card-section>
                <div class="text-h6">Confirmar Exclusão</div>
              </q-card-section>

              <q-card-section class="q-pt-none">
                Tem certeza que deseja excluir o livro "{{ deleteDialog.data.name }}"?
              </q-card-section>

              <q-card-actions align="right">
                <q-btn flat label="Excluir" color="primary" @click="confirmDelete" />
                <q-btn flat label="Cancelar" color="primary" v-close-popup />
              </q-card-actions>
            </q-card>
          </q-dialog>
        </div>
      </template>
    </TableComponents>
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
import { ref } from 'vue';
import TableComponents from '../components/TableComponents.vue';

const columns = [
  { name: 'name', required: true, label: 'Nome do Livro', align: 'center', field: row => row.name, sortable: true },
  { name: 'editora', label: 'Editora', field: 'editora', sortable: true, align: 'center' },
  { name: 'autor', label: 'Autor', field: 'autor', sortable: true, align: 'center' },
  { name: 'data', label: 'Data', field: 'data', align: 'center' },
  { name: 'actions', label: 'Ações', align: 'center' }
];

const rows = ref([
  { name: 'Livro 1', editora: 'Editora A', autor: 'João da Silva', data: '12/12/2024' },
  { name: 'Livro 2', editora: 'Editora B', autor: 'Maria de Souza', data: '01/01/2023' },
  { name: 'Livro 3', editora: 'Editora C', autor: 'Pedro Pereira', data: '15/05/2022' },
  { name: 'Livro 4', editora: 'Editora D', autor: 'Lucas Lima', data: '10/10/2021' },
  { name: 'Livro 5', editora: 'Editora E', autor: 'Ana Silva', data: '20/09/2020' },
  { name: 'Livro 6', editora: 'Editora F', autor: 'Carlos Alberto', data: '30/08/2019' },
  { name: 'Livro 7', editora: 'Editora G', autor: 'Juliana Lima', data: '15/07/2018' },
  { name: 'Livro 8', editora: 'Editora H', autor: 'Felipe Souza', data: '25/06/2017' },
  { name: 'Livro 9', editora: 'Editora I', autor: 'Renata Santos', data: '05/05/2016' }
]);

const viewDialog = ref({
  visible: false,
  data: {}
});

const editDialog = ref({
  visible: false,
  data: {}
});

const deleteDialog = ref({
  visible: false,
  data: {}
});

const openViewDialog = (row) => {
  viewDialog.value.data = row;
  viewDialog.value.visible = true;
};

const openEditDialog = (row) => {
  editDialog.value.data = { ...row };
  editDialog.value.visible = true;
};

const openDeleteDialog = (row) => {
  deleteDialog.value.data = row;
  deleteDialog.value.visible = true;
};

const saveEdit = () => {
  const index = rows.value.findIndex(r => r.name === editDialog.value.data.name);
  if (index !== -1) {
    rows.value[index] = { ...editDialog.value.data };
    editDialog.value.visible = false;
  }
};

const confirmDelete = () => {
  const index = rows.value.findIndex(r => r.name === deleteDialog.value.data.name);
  if (index !== -1) {
    rows.value.splice(index, 1);
    deleteDialog.value.visible = false;
  }
};
</script>
