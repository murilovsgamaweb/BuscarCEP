<template>
  <div class="container">
    <div class="form">
      <q-input
        filled
        v-model="formData.cep"
        label="CEP"
        outlined
        maxlength="8"
        input-class="text-white"
        :label-class="labelClass"
        @input="clearDataIfInvalid"
      />
      <q-input
        v-if="formData.nome"
        filled
        v-model="formData.nome"
        label="Nome"
        outlined
        readonly
        input-class="text-white"
        :label-class="labelClass"
      />
      <q-input
        v-for="(field, key) in filteredFields"
        :key="key"
        filled
        v-model="formData[key]"
        :label="field.label"
        outlined
        readonly
        input-class="text-white"
        :label-class="labelClass"
        class="custom-label"
      />
      <q-input
        filled
        v-model="formData.complemento"
        label="Complemento"
        outlined
        input-class="text-white"
        :label-class="labelClass"
        class="custom-label"
      />
      <q-btn label="Buscar CEP" @click="buscarCEP" />
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import { Notify } from 'quasar';

const formData = ref({
  cep: '',
  nome: '',
  logradouro: '',
  bairro: '',
  cidade: '',
  estado: '',
  complemento: '',
});

const fields = {
  logradouro: { label: 'Logradouro' },
  bairro: { label: 'Bairro' },
  cidade: { label: 'Cidade' },
  estado: { label: 'Estado' },
};

const labelClass = 'text-white';

const filteredFields = computed(() => {
  const { complemento, ...rest } = fields;
  return rest;
});

const buscarCEP = async () => {
  if (formData.value.cep.length === 8) {
    try {
      const response = await fetch(`https://servicos.remaza.com.br/api/CEP/${formData.value.cep}`);
      const data = await response.json();

      if (data.ERRO) {
        Notify.create({
          message: 'CEP não encontrado',
          color: 'negative',
        });
        resetFields();
      } else {
        formData.value.nome = data.Nome || '';
        formData.value.logradouro = data.Logradouro || '';
        formData.value.bairro = data.Bairro || '';
        formData.value.cidade = data.Cidade || '';
        formData.value.estado = data.Estado || '';
        formData.value.complemento = data.Complemento || '';

        if (!formData.value.logradouro && formData.value.bairro && formData.value.cidade) {
          formData.value.logradouro = `${formData.value.bairro}, ${formData.value.cidade}`;
        }
      }
    } catch (error) {
      Notify.create({
        message: 'Erro ao buscar o CEP',
        color: 'negative',
      });
    }
  } else {
    resetFields();
  }
};

const resetFields = () => {
  formData.value.nome = '';
  formData.value.logradouro = '';
  formData.value.bairro = '';
  formData.value.cidade = '';
  formData.value.estado = '';
  formData.value.complemento = '';
};

const clearDataIfInvalid = () => {
  if (formData.value.cep.length !== 8) {
    resetFields();
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

.custom-label {
  margin-bottom: 1rem;
}
</style>
