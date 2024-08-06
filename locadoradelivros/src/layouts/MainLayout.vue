<template>
  <q-layout view="lHh Lpr lFf" v-if="!log">
    <q-header class="transparent-header">
      <q-toolbar class="transparent-toolbar">
        <q-toolbar-title>
          <img src="../assets/altislab.png" alt="" width="150px" />
        </q-toolbar-title>

        <q-btn
          color="black"
          flat
          dense
          round
          :width="200"
          icon="account_circle"
        />
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="drawer"
      show-if-above
      :mini="miniSidebar"
      :width="200"
      :breakpoint="200"
      bordered
      style="background-color: #220127; color: white;"
    >
      <q-list>
        <q-item class="buttom">
          <q-btn
            flat
            dense
            round
            icon="menu"
            aria-label="Menu"
            @click="toggleSidebar"
          />
        </q-item>

        <div class="space"></div>
        <EssentialLink
          v-for="link in linksList"
          :key="link.title"
          v-bind="link"
        />
        <div class="space"></div>
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>

  <q-layout v-else class="centered-layout">
    <div class="form-container q-pa-md">
      <div class="form-title">
        <img src="../assets/altislab.png" alt="" width="200px" />
      </div>
      <q-form @submit="onSubmit" @reset="onReset" class="q-gutter-md">
        <q-input
          bg-color="grey-5"
          rounded
          standout
          v-model="name"
          label="Digite seu nome*"
          lazy-rules
          :rules="[val => val && val.length > 0 || 'Please type something']"
        />
        <q-input
          bg-color="grey-5"
          rounded
          standout
          type="password"
          v-model="password"
          label="Digite sua senha *"
          lazy-rules
          :rules="[
            val => val !== null && val !== '' || 'Please type your password',
            val => val.length >= 6 || 'Password must be at least 6 characters'
          ]"
        />
        <div class="btn-form">
          <q-btn rounded label="Confirmar" type="submit"  style="font-size: 14px; padding: 10px 20px; background-color: #220127; color: white;"   />
        </div>
      </q-form>
    </div>
  </q-layout>
</template>

<style scoped>
.transparent-header {
  background: transparent;
  box-shadow: none !important;
}
.transparent-toolbar {
  box-shadow: none !important;
}
.space {
  height: 150px;
}
.centered-layout {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
.form-container {
  width: 100%;
  max-width: 500px;
  padding: 2em;
  background: white;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.5);
}
.form-title
{
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 30px;
}

.btn-form{
  display: flex;
  align-items: center;
  justify-content: center;
}


</style>

<script setup>
import { ref } from 'vue'
import EssentialLink from 'components/EssentialLink.vue'
import { useQuasar } from 'quasar'

const $q = useQuasar()

const name = ref(null)
const password = ref(null)
const log = ref(true)
const drawer = ref(false)
const miniSidebar = ref(false)

const linksList = [
  {
    title: 'Dashboard',
    caption: 'quasar.dev',
    icon: 'dashboard',
    route: { name: 'dashboard' }
  },
  {
    title: 'Biblioteca',
    caption: 'github.com/quasarframework',
    icon: 'local_library',
    route: { name: 'biblioteca' }
  },
  {
    title: 'Aluguéis',
    caption: 'chat.quasar.dev',
    icon: 'sell',
    route: { name: 'aluguel' }
  },
  {
    title: 'Usuários',
    caption: 'forum.quasar.dev',
    icon: 'group',
    route: { name: 'usuario' }
  },
  {
    title: 'Locatários',
    icon: 'person',
    route: { name: 'locatarioteste' }
  },
  {
    title: 'Editoras',
    icon: 'edit',
    route: { name: 'editora' }
  }
]

function toggleSidebar() {
  miniSidebar.value = !miniSidebar.value
}

function onSubmit() {
  if (name.value === 'admin' && password.value === '12345678') {
    log.value = false
    $q.notify({
      color: 'green-4',
      textColor: 'white',
      icon: 'cloud_done',
      message: 'Login successful'
    })
  } else {
    $q.notify({
      color: 'red-5',
      textColor: 'white',
      icon: 'warning',
      message: 'Invalid credentials'
    })
  }
}

function onReset() {
  name.value = null
  password.value = null
}
</script>
