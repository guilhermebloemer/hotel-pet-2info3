<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink, useRouter } from 'vue-router';

const API_URL = 'http://localhost:3000';
const router = useRouter();
const tutores = ref([]);

    async function carregarTutores() {
      const respostaTutores = await fetch(`${API_URL}/tutores`);
     console.log('tutores, resposta');
      tutores.value = await respostaTutores.json();}
      
const novoPet = ref({
  nome: '',
  especie: '',
  tutorId: '',
}); 

async function salvarPet() {
await fetch(`${API_URL}/pets`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(novoPet.value),
});
router.push('/pets');  
}

-
+
onMounted(carregarTutores);
</script>

<template>  
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem de Pet</h1>
      <p class="text-body-secondary mb-0">Cadastro de Pets no sistema.</p>
    </header>
    <!--<RouterLink
      class="btn btn-primary"
      :to="{ name: 'addPet' }"
    >
      Adicionar pet
    </RouterLink>

    -->
    </div>
    <form @submit.prevent="salvarPet">
      <div class="col-md-6 mb-3 ">
        <label for="nome" class="form-label">Nome do Pet:</label>
        <input
          type="text"
          id="nome"
          v-model="novoPet.nome"
          class="form-control"
          required
        />
      </div>
        <div class="col-md-6 mb-3">
            <label for="especie" class="form-label">Espécie:</label>
            <input
            type="text"
            id="especie"
            v-model="novoPet.especie"
            class="form-control"
            required
            />
        </div>
     

      

      <button type="submit" class="btn btn-primary">Salvar</button>
    </form>

</template>