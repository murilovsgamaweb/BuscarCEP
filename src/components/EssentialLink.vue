<template>
  <div class="container">
    <q-item
      clickable
      tag="a"
      target="_blank"
      :href="props.link"
      class="custom-link"
    >
      <q-item-section v-if="props.icon" avatar class="icon-section">
        <q-icon :name="props.icon" />
      </q-item-section>

      <q-item-section class="text-section">
        <q-item-label>{{ props.title }}</q-item-label>
        <q-item-label caption>{{ props.caption }}</q-item-label>
      </q-item-section>
    </q-item>
  </div>
</template>

<script setup>
const props = defineProps({
  title: {
    type: String,
    required: true,
  },

  caption: {
    type: String,
    default: "",
  },

  link: {
    type: String,
    default: "#",
  },

  icon: {
    type: String,
    default: "",
  },
});
</script>

<style scoped>
.container {
  background-color: #1e1e1e; /* Fundo escuro para a navbar */
  padding: 0.5rem; /* Espaçamento ao redor */
  display: flex;
  align-items: center;
  height: 13.4vh;
  justify-content: center;
  border-bottom: 1px solid #0f0850; /* Borda inferior azul escura */
}

.custom-link {
  background-color: #2c2c2c; /* Cor de fundo mais escura */
  border: 1px solid #0f0850; /* Borda azul escura */
  color: #e0e0e0; /* Cor do texto clara */
  border-radius: 8px; /* Bordas arredondadas */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3); /* Sombra */
  padding: 0.5rem 1rem; /* Espaçamento interno */
  margin: 0.5rem; /* Espaçamento externo */
  transition: background-color 0.3s, box-shadow 0.3s; /* Transição suave para hover */
}

.custom-link:hover {
  background-color: #3a3a3a; /* Cor de fundo ao passar o mouse */
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.4); /* Sombra mais forte ao passar o mouse */
}

.icon-section {
  margin-right: 0.5rem; /* Espaçamento entre o ícone e o texto */
}

.text-section {
  color: #e0e0e0; /* Cor do texto */
}

.q-item-label {
  color: #e0e0e0; /* Cor do texto branca */
}
</style>
