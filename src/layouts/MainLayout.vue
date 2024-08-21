<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />

        <q-toolbar-title>
          Busca de CEP
        </q-toolbar-title>

        <div>Quasar v{{ $q.version }}</div>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
      class="drawer"
    >
      <q-list>
        <q-item-label
          header
          class="drawer-header"
        >
          Links Importantes
        </q-item-label>

        <EssentialLink
          v-for="link in linksList"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup>
import { ref } from 'vue'
import EssentialLink from 'components/EssentialLink.vue'

defineOptions({
  name: 'MainLayout'
})

const linksList = [
  {
    title: 'Documentação',
    caption: 'saiba como usar a API',
    icon: 'book',
    link: 'https://example.com/docs'
  },
  {
    title: 'Sobre o Projeto',
    caption: 'Informações sobre a busca de CEP',
    icon: 'info',
    link: 'https://example.com/about'
  },
  {
    title: 'Suporte',
    caption: 'Entre em contato',
    icon: 'support',
    link: 'https://example.com/support'
  }
]

const leftDrawerOpen = ref(false)

function toggleLeftDrawer () {
  leftDrawerOpen.value = !leftDrawerOpen.value
}
</script>

<style scoped>
.q-header {
  background-color: #1e1e1e;
  color: #f5f5f5;
}

.q-toolbar {
  background-color: #1e1e1e;
  color: #f5f5f5;
}

.q-drawer {
  background-color: #2c2c2c;
}

.drawer-header {
  color: #f5f5f5;
  font-size: 1.2rem;
  font-weight: bold;
  background-color: #1e1e1e;
}

.drawer .q-item {
  border-radius: 8px;
  margin: 0.5rem 0;
  background-color: #1e1e1e;
  color: #f5f5f5;
}

.drawer .q-item:hover {
  background-color: #333;
}

.drawer .q-item-label {
  color: #f5f5f5;
}

.q-page-container {
  background-color: #444444a4;
  color: #f5f5f5;
}
</style>
