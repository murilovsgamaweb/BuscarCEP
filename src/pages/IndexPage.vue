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
        v-if="nome"
        filled
        v-model="nome"
        label="Nome"
        outlined
        readonly
        input-class="text-white"
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
const nome = ref(""); 
const logradouro = ref("");
const complemento = ref("");
const bairro = ref("");
const cidade = ref("");
const estado = ref("");
const labelClass = "text-white";

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

        nome.value = "";
        logradouro.value = "";
        complemento.value = "";
        bairro.value = "";
        cidade.value = "";
        estado.value = "";
      } else {
        nome.value = data.Nome || "";
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
    nome.value = ""; 
    logradouro.value = "";
    complemento.value = "";
    bairro.value = "";
    cidade.value = "";
    estado.value = "";
  }
};
</script>

<style scoped>
.container {
  background-color: #0000006e; 
  min-height: 100vh; 
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem;
}

.form {
  background-color: #1e1e1e7e; 
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
  width: 100%;
  max-width: 400px; 
}
.q-input,
.q-btn {
  border-radius: 4px;
  border: 1px solid #0f0850;
  margin-bottom: 1rem;
  background-color: #62626283;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
  color: white;
}

.q-btn {
  width: 100%;
  background: #0f0850;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
  color: #f5f5f5; 
  font-size: 1rem; 
  font-weight: 400; 
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  border-radius: 6px;
}

.q-input .q-field__label {
  color: white;
}
</style>
