<template>
  <q-layout view="lHh Lpr lFf" >
    <q-header  class="transparent-header">
      <q-toolbar class="transparent-toolbar" >


        <q-toolbar-title>
          <img src="../assets/altislab.png" alt="" width="150px" >
        </q-toolbar-title>


      </q-toolbar>
    </q-header>


    <q-drawer
      v-model="drawer"
      show-if-above
      :mini="miniSidebar"
      :width="250"
      :breakpoint="500"
      bordered
      style="background-color: #220127; color: white;"
    >
      <q-list >
        <q-item class="buttom ">
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleSidebar"
        />
        </q-item>

        <div class="space" ></div>
          <EssentialLink
            v-for="link in linksList"
            :key="link.title"
            v-bind="link"
          />
          <div class="space" ></div>

      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<style scoped>

.transparent-header{
  background: transparent;
  box-shadow: none !important;
}
.transparent-toolbar {

  box-shadow: none !important;
}

.space{
  height: 150px;

}

</style>


<script setup>
import { ref } from 'vue'
import EssentialLink from 'components/EssentialLink.vue'

defineOptions({
  name: 'MainLayout'
})



const linksList = [
  {
    title: 'Dashboard',
    caption: 'quasar.dev',
    icon: 'dashboard',
    route: {name: 'dashboard'}
  },
  {
    title: 'Biblioteca',
    caption: 'github.com/quasarframework',
    icon: 'book',
    link: 'https://github.com/quasarframework'
  },
  {
    title: 'Discord Chat Channel',
    caption: 'chat.quasar.dev',
    icon: 'chat',
    link: 'https://chat.quasar.dev'
  },
  {
    title: 'Forum',
    caption: 'forum.quasar.dev',
    icon: 'record_voice_over',
    link: 'https://forum.quasar.dev'
  },
  {
    title: 'Twitter',
    caption: '@quasarframework',
    icon: 'rss_feed',
    link: 'https://twitter.quasar.dev'
  },
  {
    title: 'Facebook',
    caption: '@QuasarFramework',
    icon: 'public',
    link: 'https://facebook.quasar.dev'
  },
  {
    title: 'Quasar Awesome',
    caption: 'Community Quasar projects',
    icon: 'favorite',
    link: 'https://awesome.quasar.dev'
  }
]

const miniSidebar = ref(false)

function toggleSidebar () {
  miniSidebar.value = !miniSidebar.value
}
</script>
