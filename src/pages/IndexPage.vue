<template>
  <div class="q-pa-md">
    <div class="q-gutter-md" style="max-width: 300px">
      
        <q-input outlined v-model="text" label="CEP" />
        <q-input outlined readonly v-model="text" label="Cidade" />
        <q-input outlined readonly v-model="text" label="Estado" />
        <q-input outlined readonly v-model="text" label="Bairro" />
        <q-input outlined readonly v-model="text" label="Logradouro" />
        <q-input outlined v-model="text" label="Complemento" />
      </div>
  
  </div>
</template>

<script setup>
defineOptions({
  name: "IndexPage",
});
</script>

<style scoped>


.camposCEP q-input {
  width: 80%;
  background: red;
  border: 5rem solid red;
  color: red;
}
</style>
