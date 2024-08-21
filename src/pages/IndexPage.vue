<template>
  <div class="container">
    <div class="form">
      <q-input
        filled
        v-model="cep"
        label="CEP"
        input-class="text-white"
        outlined
        maxlength="8"
        :label-class="labelClass"
      />
      <q-input
        filled
        v-model="logradouro"
        label="Logradouro"
        outlined
        readonly
        input-class="text-white"
        :label-class="labelClass"
      />
      <q-input
        filled
        v-model="bairro"
        class="custom-label"
        label="Bairro"
        outlined
        readonly
        input-class="text-white"
        :label-class="labelClass"
      />
      <q-input
        filled
        v-model="cidade"
        class="custom-label"
        label="Cidade"
        outlined
        readonly
        input-class="text-white"
        :label-class="labelClass"
      />
      <q-input
        filled
        v-model="estado"
        class="custom-label"
        label="Estado"
        outlined
        readonly
        autofocus
        :label-class="labelClass"
        input-class="text-white"
      />
      <q-input
        filled
        v-model="complemento"
        class="custom-label"
        label="Complemento"
        outlined
        input-class="text-white"
        :label-class="labelClass"
      />
      <q-btn label="Buscar CEP" @click="buscarCEP" />
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { Notify } from "quasar";

const cep = ref("");
const logradouro = ref("");
const complemento = ref("");
const bairro = ref("");
const cidade = ref("");
const estado = ref("");

// Define a classe para os labels dos inputs
const labelClass = 'text-white';

const buscarCEP = async () => {
  if (cep.value.length === 8) {
    try {
      const response = await fetch(
        `https://servicos.remaza.com.br/api/CEP/${cep.value}`
      );
      const data = await response.json();

      if (data.ERRO) {
        Notify.create({
          message: "CEP não encontrado",
          color: "negative",
        });

        logradouro.value = "";
        complemento.value = "";
        bairro.value = "";
        cidade.value = "";
        estado.value = "";
      } else {
        logradouro.value = data.Logradouro || "";
        complemento.value = data.Complemento || "";
        bairro.value = data.Bairro || "";
        cidade.value = data.Cidade || "";
        estado.value = data.Estado || "";

        if (!logradouro.value && bairro.value && cidade.value) {
          logradouro.value = `${bairro.value}, ${cidade.value}`;
        }
      }
    } catch (error) {
      Notify.create({
        message: "Erro ao buscar o CEP",
        color: "negative",
      });
    }
  } else {
    logradouro.value = "";
    complemento.value = "";
    bairro.value = "";
    cidade.value = "";
    estado.value = "";
  }
};
</script>

<style scoped>
/* Estilo para a tela inteira com fundo escuro */
.container {
  background-color: #0000006e; /* Cor de fundo escura */
  min-height: 100vh; /* Altura mínima para cobrir toda a tela */
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem;
}

.form {
  background-color: #1e1e1e7e; /* Cor de fundo do formulário */
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
  width: 100%;
  max-width: 400px; /* Largura máxima do formulário */
}

/* Estilo dos inputs e botão para melhor visibilidade */
.q-input,
.q-btn {
  border-radius: 4px;
  border: 1px solid #0f0850;
  margin-bottom: 1rem;
  background-color: #1e1e1e34; /* Cor de fundo dos inputs */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
  color: white;
}

.q-btn {
  width: 100%;
  background: #0f0850;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
  color: #f5f5f5; /* Cor da label do botão */
  font-size: 1rem; /* Tamanho da fonte da label do botão */
  font-weight: 400; /* Peso da fonte da label do botão */
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  border-radius: 6px;
}

/* Cor dos labels dos inputs usando o Quasar */
.q-input .q-field__label {
  color: white;
}
</style>
