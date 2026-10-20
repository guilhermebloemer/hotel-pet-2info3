<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink, useRoute } from 'vue-router';

const API_URL = 'http://localhost:3000';
const route = useRoute();
const tutor = ref({});
const pets = ref([]);
const novoPet = ref({
  nome: '',
  especie: '',
});

async function carregarDados() {
  try {
    const [respostaTutor, respostaPets] = await Promise.all([
      fetch(`${API_URL}/tutores/${route.params.id}`),
      fetch(`${API_URL}/pets?tutorId=${route.params.id}`),
    ]);

    if (!respostaTutor.ok || !respostaPets.ok) {
      throw new Error('Erro ao carregar os dados do tutor e dos pets');
    }

    [tutor.value, pets.value] = await Promise.all([
      respostaTutor.json(),
      respostaPets.json(),
    ]);
  } catch (error) {
    console.error('Erro ao carregar dados do tutor:', error);
    alert('Não foi possível carregar os dados do tutor.');
  }
}

async function cadastrarPet() {
  try {
    const resposta = await fetch(`${API_URL}/pets`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        ...novoPet.value,
        tutorId: route.params.id,
      }),
    });

    if (!resposta.ok) {
      throw new Error('Erro ao cadastrar pet');
    }

    novoPet.value = { nome: '', especie: '' };
    await carregarDados();
  } catch (error) {
    console.error('Erro ao cadastrar pet:', error);
    alert('Não foi possível cadastrar o pet.');
  }
}

onMounted(carregarDados);
</script>

<template>
  <div>
    <header class="mb-4 d-flex justify-content-between align-items-center">
      <div>
        <h1 class="text-2xl fw-bold">Detalhes do Tutor</h1>
        <p class="text-body-secondary mb-0">
          Visualize os dados do tutor e seus pets.
        </p>
      </div>
      <RouterLink
        class="btn btn-outline-primary"
        :to="{ name: 'tutores' }"
      >
        Voltar para Tutores
      </RouterLink>
    </header>

    <section class="card mb-4">
      <div class="card-header">
        <h2 class="h5 mb-0">Dados do Tutor</h2>
      </div>
      <div class="card-body row g-3">
        <p class="col-md-4 mb-0"><strong>ID:</strong> {{ tutor.id }}</p>
        <p class="col-md-4 mb-0"><strong>Nome:</strong> {{ tutor.nome }}</p>
        <p class="col-md-4 mb-0"><strong>CPF:</strong> {{ tutor.cpf }}</p>
        <p class="col-md-4 mb-0">
          <strong>Telefone:</strong> {{ tutor.telefone }}
        </p>
        <p class="col-md-4 mb-0"><strong>E-mail:</strong> {{ tutor.email }}</p>
        <p class="col-md-4 mb-0">
          <strong>Data de nascimento:</strong> {{ tutor.dataNascimento }}
        </p>
      </div>

      <div class="card-header">
        <h2 class="h5 mb-0">Endereço</h2>
      </div>
      <div class="card-body row g-3">
        <p class="col-md-4 mb-0"><strong>Rua:</strong> {{ tutor.rua }}</p>
        <p class="col-md-4 mb-0"><strong>Número:</strong> {{ tutor.numero }}</p>
        <p class="col-md-4 mb-0"><strong>Bairro:</strong> {{ tutor.bairro }}</p>
        <p class="col-md-4 mb-0"><strong>Cidade:</strong> {{ tutor.cidade }}</p>
        <p class="col-md-4 mb-0"><strong>Estado:</strong> {{ tutor.estado }}</p>
        <p class="col-md-4 mb-0"><strong>CEP:</strong> {{ tutor.cep }}</p>
      </div>
    </section>

    <section class="card">
      <div class="card-header">
        <h2 class="h5 mb-0">Pets do Tutor ({{ pets.length }})</h2>
      </div>
      <div class="card-body">
        <table class="table table-striped">
      <thead>
        <tr>
          <th>ID</th>
          <th>Nome</th>
          <th>Espécie</th>
          <th>Ação</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="pet in pets"
          :key="pet.id"
        >
          <td>{{ pet.id }}</td>
          <td>{{ pet.nome }}</td>
          <td>{{ pet.especie }}</td>
          <td>
            <RouterLink
              :to="{ name: 'detalhes-pet', params: { id: pet.id } }"
            >
              Visualizar
            </RouterLink>
          </td>
        </tr>
        <tr v-if="pets.length === 0">
          <td colspan="4">Nenhum pet cadastrado para este tutor.</td>
        </tr>
      </tbody>
        </table>

        <h2 class="h5">Cadastrar novo pet</h2>
        <p class="text-body-secondary">
          O pet será vinculado automaticamente a {{ tutor.nome }}.
        </p>
        <form
          class="row g-3"
          @submit.prevent="cadastrarPet"
        >
          <div class="col-md-5">
            <label
              for="nomePet"
              class="form-label"
            >
              Nome do pet
            </label>
            <input
              id="nomePet"
              v-model="novoPet.nome"
              class="form-control"
              type="text"
              required
            />
          </div>

          <div class="col-md-5">
            <label
              for="especiePet"
              class="form-label"
            >
              Espécie
            </label>
            <select
              id="especiePet"
              v-model="novoPet.especie"
              class="form-select"
              required
            >
              <option
                value=""
                disabled
              >
                Selecione
              </option>
              <option value="Cachorro">Cachorro</option>
              <option value="Gato">Gato</option>
            </select>
          </div>

          <div class="col-md-2 d-flex align-items-end">
            <button
              class="btn btn-success w-100"
              type="submit"
            >
              Cadastrar
            </button>
          </div>
        </form>
      </div>
    </section>
  </div>
</template>
