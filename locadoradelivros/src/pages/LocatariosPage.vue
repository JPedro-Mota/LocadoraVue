<template>
  <div class="title">
    <h6>
      <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960" width="30px" fill="#000000"><path d="M480-480q-66 0-113-47t-47-113q0-66 47-113t113-47q66 0 113 47t47 113q0 66-47 113t-113 47ZM160-160v-112q0-34 17.5-62.5T224-378q62-31 126-46.5T480-440q66 0 130 15.5T736-378q29 15 46.5 43.5T800-272v112H160Z"/></svg>
      Locatário
    </h6>
  </div>
  <q-page padding>
    <div class="tableHeader">
      <q-input bg-color="grey-4" rounded standout dense bottom-slots v-model="text" label="Pesquisar" class="input-field">
        <template v-slot:prepend>
          <q-icon name="search" @click=" getTable(text)" />
        </template>
        <template v-slot:append>
          <q-icon name="close" @click="text = '', getTable(text)" class="cursor-pointer" />
        </template>
      </q-input>
      <q-btn rounded dense icon="add" label="Criar" @click="openCreatDialog" color="green" class="button-field"></q-btn>
    </div>
    <TableComponents :columns="columns" :rows="filteredRows">
      <template #actions="{ row }">
        <div class="dialogsa">
          <q-btn flat round dense icon="visibility" @click="openViewDialog(row)" class="actions-bt" />
          <q-btn flat round dense icon="edit" @click="openEditDialog(row)" class="actions-bt" />
          <q-btn flat round dense icon="delete" @click="openDeleteDialog(row)" class="actions-bt" />


          <q-dialog v-model="viewDialog.visible" persistent>
            <q-card>
              <q-card-section>
                <div class="text-h6">Detalhes da Editora</div>
              </q-card-section>
              <q-card-section class="q-pt-none">
                <div><strong>Id:</strong> {{ InfosEdit.id }}</div>
                <div><strong>Nome:</strong> {{ InfosEdit.name }}</div>
                <div><strong>Email:</strong> {{ InfosEdit.email }}</div>
                <div><strong>Telephone:</strong> {{ InfosEdit.telephone }}</div>
                <div><strong>Endereço:</strong> {{ InfosEdit.address}}</div>
                <div><strong>CPF:</strong> {{ InfosEdit.cpf }}</div>
              </q-card-section>
              <q-card-actions align="right">
                <q-btn flat label="Fechar" color="primary" v-close-popup />
              </q-card-actions>
            </q-card>
          </q-dialog>


          <q-dialog v-model="editDialog.visible" persistent>
            <q-card>
              <q-card-section>
                <div class="text-h6">Editar Editora</div>
              </q-card-section>
              <q-card-section class="q-pt-none">
                <q-input v-model="InfosEdit.id" label="Id" />
                <q-input v-model="InfosEdit.name" label="Nome" />
                <q-input v-model="InfosEdit.email" label="E-mail" />
                <q-input v-model="InfosEdit.telephone" label="Telefone" />
                <q-input v-model="InfosEdit.address" label="Endereço " />
                <q-input v-model="InfosEdit.cpf" label="CPF " />
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
                Tem certeza que deseja excluir a editora "{{ deleteDialog.data.name }}"?
              </q-card-section>
              <q-card-actions align="right">
                <q-btn flat label="Excluir" color="primary" @click="confirmDelete" />
                <q-btn flat label="Cancelar" color="primary" v-close-popup />
              </q-card-actions>
            </q-card>
          </q-dialog>

          <q-dialog v-model="creatDialog.visible" persistent>
            <q-card>
              <q-card-section>
                <div class="text-h6">Cadastrar Editora</div>
              </q-card-section>
              <q-card-section class="q-pt-none">
                <q-input v-model="newRenter.name" label="Nome" />
                <q-input v-model="newRenter.email" label="Email" />
                <q-input v-model="newRenter.telephone" label="Telefone" />
                <q-input v-model="newRenterr.site" label="Site" />
              </q-card-section>
              <q-card-actions align="right">
                <q-btn flat label="Salvar" color="primary" @click="saveNewPublisher" />
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
.tableHeader {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.input-field {
  flex: 1;
}

.button-field {
  margin-left: 10px;
  padding: 7px;
  margin-bottom: 2%;
}
</style>

<script setup>
import { onMounted, ref, computed } from 'vue';
import TableComponents from '../components/TableComponents.vue';
import { api, authenticate } from 'src/boot/axios';

onMounted(() => {
  authenticate()
    .then(() => {
      console.log("Sucessou");
      getTable();
    })
    .catch(error => {
      console.error("Erro na autenticação", error);
    });
});

const columns = [
  { name: 'name', required: true, label: 'Nome', align: 'center', field: row => row.name, sortable: true },
  { name: 'actions', label: 'Ações', align: 'center' }
];

const rows = ref([]);
const text = ref('');

const getTable = (inputSearch = '') => {
  api.get('/renter', {params: {search: inputSearch}})
    .then(response => {
      if (Array.isArray(response.data.content)) {
        rows.value = response.data.content;
        console.log("Dados obtidos com sucesso");
      } else {
        console.error('A resposta da API não é um array:', response.data);
        rows.value = [];
      }
      console.log('Resposta da API:', response.data);
    })
    .catch(error => {
      console.error("Erro ao obter dados:", error);
    });
}

const InfosEdit = ref({});
const newPublisher = ref({ name: '', email: '', telephone: '', site: '' });

const getApi = (id) => {
  api.get(`/renter/${id}`)
    .then(response => {
      InfosEdit.value = response.data;
      console.log(InfosEdit.value);
    })
    .catch(error => {
      console.error("Erro", error);
    });
}

const viewDialog = ref({
  visible: false,
  data: {},
});

const editDialog = ref({
  visible: false,
  data: {}
});

const deleteDialog = ref({
  visible: false,
  data: {}
});

const creatDialog = ref({
  visible: false,
  data: {}
});

const openViewDialog = (row) => {
  getApi(row.id);
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

const openCreatDialog = () => {
  newRenter.value = { name: '', email: '', telephone: '', address: '', cpf: '' };
  creatDialog.value.visible = true;
}

const saveEdit = () => {
  const index = rows.value.findIndex(r => r.id === editDialog.value.data.id);
  if (index !== -1) {
    api.put(`/renter/${editDialog.value.data.id}`, editDialog.value.data)
      .then(() => {
        rows.value[index] = { ...editDialog.value.data };
        editDialog.value.visible = false;
      })
      .catch(error => {
        console.error("Erro ao salvar edição:", error);
      });
  }
};

const confirmDelete = () => {
  const index = rows.value.findIndex(r => r.id === deleteDialog.value.data.id);
  if (index !== -1) {
    api.delete(`/renter/${deleteDialog.value.data.id}`)
      .then(() => {
        rows.value.splice(index, 1);
        deleteDialog.value.visible = false;
      })
      .catch(error => {
        console.error("Erro ao excluir:", error);
      });
  }
};

const saveNewPublisher = () => {
  api.post('/renter', newPublisher.value)
    .then(response => {
      rows.value.push(response.data);
      creatDialog.value.visible = false;
    })
    .catch(error => {
      console.error("Erro ao salvar nova editora:", error);
    });
};

const filteredRows = computed(() => {
  const searchText = text.value.toLowerCase();
  return rows.value.filter(row =>
    row.name.toLowerCase().includes(searchText) ||
    row.email.toLowerCase().includes(searchText) ||
    row.telephone.toLowerCase().includes(searchText) ||
    row.site.toLowerCase().includes(searchText)
  );
});
</script>
