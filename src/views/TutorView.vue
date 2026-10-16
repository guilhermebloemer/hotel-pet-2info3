<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink } from 'vue-router';

const API_URL = 'http://localhost:3000';
const tutores = ref([]);

async function carregarDados() {
  const resposta = await fetch(`${API_URL}/tutores`);
  tutores.value = await resposta.json();
}

async function excluirTutor(tutorId) {
  const confirmarExclusao = window.confirm(
    'Tem certeza que deseja excluir este tutor?',
  );

  if (!confirmarExclusao) {
    return;
  }

  try {
    const resposta = await fetch(`${API_URL}/tutores/${tutorId}`, {
      method: 'DELETE',
    });

    if (!resposta.ok) {
      throw new Error('Erro ao excluir tutor');
    }

    await carregarDados();
  } catch (error) {
    console.error('Erro ao excluir tutor:', error);
    alert('Não foi possível excluir o tutor.');
  }
}

onMounted(carregarDados);
</script>

<template>
  <div>
    <header class="mb-4 d-flex justify-content-between align-items-center">
      <div>
        <h1 class="text-2xl font-bold">Listagem de Tutores</h1>
        <p class="text-body-secondary mb-0">Tutores cadastrados no sistema.</p>
      </div>
      <RouterLink
        class="btn btn-primary"
        :to="{ name: 'addTutor' }"
        >Novo tutor</RouterLink
      >
    </header>

    <table class="table table-striped table-hover">
      <thead>
        <tr>
          <th>ID</th>
          <th>Nome</th>
          <th>CPF</th>
          <th>Telefone</th>
          <th>Email</th>
          <th>Quantidade de Animais</th>
          <th>Nome dos Pets</th>  
        </tr>
      </thead>

      <tbody>
        <tr
          v-for="tutor in tutores"
          :key="tutor.id"
        >
          <td>{{ tutor.id }}</td>
          <td>{{ tutor.nome }}</td>
          <td>{{ tutor.cpf }}</td>
          <td>{{ tutor.telefone }}</td>
          <td>{{ tutor.email }}</td>
          <td>{{ tutor.quantidade_animais }}</td>
          <td>{{ tutor.nome_dos_pets }}</td>
          <td>
            <RouterLink
              class="btn btn-sm btn-primary me-2"
              :to="{ name: 'detalhes-tutor', params: { id: tutor.id } }"
            >
              Visualizar
            </RouterLink>
            <button
              class="btn btn-sm btn-danger"
              @click="excluirTutor(tutor.id)"
            >
              Excluir
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
