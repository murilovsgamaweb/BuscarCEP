<template>
  <div>
    <q-input
      filled
      v-model="cep"
      label="CEP"
      outlined
      maxlength="8"
    />
    <q-input
    filled
    v-model="logradouro"
    label="Logradouro"
    outlined
    readonly
    />
    <q-input
    filled
    v-model="bairro"
    label="Bairro"
    outlined
    readonly
    />
    <q-input
    filled
    v-model="cidade"
    label="Cidade"
      outlined
      readonly
      />
      <q-input
      filled
      v-model="estado"
      label="Estado"
      outlined
      readonly
      />
      <q-input
      filled
      v-model="complemento"
      label="Complemento"
      outlined
      />
    </div>
    <q-btn
      label="Buscar CEP"
      color="primary"
      @click="buscarCEP"
    />
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
        // Limpar campos em caso de erro
        logradouro.value = "";
        complemento.value = "";
        bairro.value = "";
        cidade.value = "";
        estado.value = "";
      } else {
        logradouro.value = data.Logradouro;
        complemento.value = data.Complemento;
        bairro.value = data.Bairro;
        cidade.value = data.Cidade;
        estado.value = data.Estado;

        // Exibindo todos os valores no console
        console.log({
          cep: cep.value,
          logradouro: logradouro.value,
          complemento: complemento.value,
          bairro: bairro.value,
          cidade: cidade.value,
          estado: estado.value
        });
      }
    } catch (error) {
      Notify.create({
        message: "Erro ao buscar o CEP",
        color: "negative",
      });
    }
  } else {
    // Limpar campos se o CEP tiver menos de 8 caracteres
    logradouro.value = "";
    complemento.value = "";
    bairro.value = "";
    cidade.value = "";
    estado.value = "";
  }
};
</script>

<style scoped>
/* Adicione estilos personalizados aqui, se necessário */
</style>
