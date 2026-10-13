<script setup>
import { onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';

const API_URL = 'http://localhost:3000';
const tutor = ref({});
const route = useRoute();

async function carregarTutor() {
  try {
    const resposta = await fetch(`${API_URL}/tutores/${route.params.id}`);

    if (!resposta.ok) {
      throw new Error(`Erro ao carregar tutor: ${resposta.status}`);
    }

    tutor.value = await resposta.json();
  } catch (error) {
    console.error('Erro ao carregar dados do tutor:', error);
  }
}

onMounted(carregarTutor);
</script>

<template>
  <div>
    <h1>Nome: {{ tutor.nome }}</h1>
    <p>Telefone: {{ tutor.telefone }}</p>
  </div>
</template>
