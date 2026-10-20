<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';

const API_URL = 'http://localhost:3000';
const router = useRouter();

const novoTutor = ref({
  id: '',
  telefone: '',
  cpf: '',
  email: '',
  quantidade_animais: 0,
  nome_dos_pets: '',
  dataNascimento: '',
  rua: '',
  numero: '',
  bairro: '',
  cidade: '',
  estado: '',
  cep: '',
});

const cepApi = ref({});

async function buscaCep(cep) {
  const API_CEP = 'https://viacep.com.br/ws';
  const respostaCep = await fetch(`${API_CEP}/${cep}/json/`);
  cepApi.value = await respostaCep.json();

  novoTutor.value.rua = cepApi.value.logradouro;
  novoTutor.value.bairro = cepApi.value.bairro;
  novoTutor.value.cidade = cepApi.value.localidade;
  novoTutor.value.estado = cepApi.value.uf;
}

async function salvarTutor() {
  await fetch(`${API_URL}/tutores`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(novoTutor.value),
  });

  router.push('/tutores');
}


</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Cadastro de Tutores</h1>
      <p class="text-body-secondary mb-0">Cadastro de tutores no sistema.</p>
    </header>
    <h2>Dados do Tutor</h2>

    <form
      @submit.prevent="salvarTutor"
      class="row g-3"
    >
      <div class="col-md-6">
        <label
          for="idTutor"
          class="form-label"
        >
          ID do Tutor
        </label>
        <input
          id="idTutor"
          v-model="novoTutor.id"
          type="text"
          class="form-control"
          placeholder="Gerado automaticamente"
          readonly
        />
      </div>

      <div class="col-md-6">
        <label
          for="nome"
          class="form-label"
          >Nome do tutor:</label
        >
        <input
          id="nome"
          v-model="novoTutor.nome"
          type="text"
          class="form-control"
          required
          placeholder="Digite seu nome"
        />
      </div>

      <div class="col-md-6">
        <label
          for="cpf"
          class="form-label"
          >CPF:</label
        >
        <input
          id="cpf"
          v-model="novoTutor.cpf"
          type="text"
          class="form-control"
          required
          placeholder="Digite seu CPF"
        />
      </div>

      <div class="col-md-6">
        <label
          for="email"
          class="form-label"
          >Email do tutor:</label
        >
        <input
          id="email"
          v-model="novoTutor.email"
          type="email"
          class="form-control"
          required
          placeholder="Digite seu email"
        />
      </div>

      <div class="col-md-6">
        <label
          for="telefone"
          class="form-label"
          >Telefone:</label
        >
        <input
          id="telefone"
          v-model="novoTutor.telefone"
          type="text"
          class="form-control"
          required
          placeholder="(xx) xxxxx-xxxx"
        />
      </div>
      <div class="col-md-6">
        <label
          for="dataNascimento"
          class="form-label"
          >Data de Nascimento:</label
        >
        <input
          id="dataNascimento"
          v-model="novoTutor.dataNascimento"
          type="text"
          class="form-control"
          required
          placeholder="dd/mm/aaaa"
        />
      </div>

      <h2>Endereço</h2>

      <div class="col-md-6">
        <label
          for="rua"
          class="form-label"
          >Rua</label
        >
        <input
          id="rua"
          v-model="novoTutor.rua"
          type="text"
          class="form-control"
          required
          placeholder="Digite o nome da rua"
        />
      </div>

      <div class="col-md-6">
        <label
          for="numero"
          class="form-label"
          >Número</label
        >
        <input
          id="numero"
          v-model="novoTutor.numero"
          type="text"
          class="form-control"
          required
          placeholder="Digite o número"
        />
      </div>
      <div class="col-md-6">
        <label
          for="bairro"
          class="form-label"
          >Bairro</label
        >
        <input
          id="bairro"
          v-model="novoTutor.bairro"
          type="text"
          class="form-control"
          required
          placeholder="Digite o nome do bairro"
        />
      </div>

      <div class="col-md-6">
        <label
          for="cidade"
          class="form-label"
          >Cidade</label
        >
        <input
          id="cidade"
          v-model="novoTutor.cidade"
          type="text"
          class="form-control"
          required
          placeholder="Digite o nome da cidade"
        />
      </div>

      <div class="col-md-6">
        <label
          for="estado"
          class="form-label"
          >Estado</label
        >
        <div class="position-relative">
          <select
            id="estado"
            v-model="novoTutor.estado"
            class="form-control pe-5"
            required
          >
            <option value="">Selecione</option>
            <option value="SP">SP</option>
            <option value="RJ">RJ</option>
            <option value="MG">MG</option>
            <option value="PR">PR</option>
            <option value="SC">SC</option>
            <option value="RS">RS</option>
            <option value="BA">BA</option>
            <option value="PE">PE</option>
            <option value="CE">CE</option>
            <option value="GO">GO</option>
          </select>
          <span
            class="position-absolute top-50 end-0 translate-middle-y me-3 text-secondary"
            style="pointer-events: none"
            >▾</span
          >
        </div>
      </div>

      <div class="col-md-6">
        <label
          for="cep"
          class="form-label"
          >CEP</label
        >
        <input
  id="cep"
  v-model="novoTutor.cep"
  type="text"
  class="form-control"
  required
  placeholder="Digite o CEP"
  @blur="buscaCep(novoTutor.cep)"
/>

      </div>

      <button
        type="submit"
        class="btn btn-success"
      >
        Salvar Tutor
      </button>
    </form>
  </div>
</template>
