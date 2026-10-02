<script setup>
import { onMounted, ref } from 'vue';
import {RouterLink, useRoute} from 'vue-router';

const API_URL = 'http://localhost:3000';
const pet = ref({});
const tutor = ref({});
const route = useRoute();


async function carregarPet() {
 try {
  const respostaPet = await fetch(`${API_URL}/pets/${route.params.id}`);
  if (!respostaPet.ok) {
    console.log('Erro ao carregar os dados do pet:', respostaPet.statusText);
  }

  pet.value = await respostaPet.json();

  const respostaTutor = await fetch(`${API_URL}/tutores/${pet.value.tutorId}`);
  tutor.value = await respostaTutor.ok? await respostaTutor.json() : {nome: 'Não especificado'};
}  catch (error) {
  console.error('Erro ao carregar os dados do pet:', error);
}
}

onMounted(carregarPet);
</script>

<template>
  <h1>Nome: {{ pet.nome }}</h1>
  <p>Espécie: {{ pet.especie }}</p>
 <!-- <p>Tutor: {{nomeDoTutor(pet.tutorId) }}</p> -->
</template>
