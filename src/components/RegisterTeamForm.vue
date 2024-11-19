<template>
  <form @submit.prevent="handleSubmit" class="space-y-4">
    <div>
      <label
        for="nome"
        class="block text-sm font-bold text-gray-700 uppercase"
        >Nome</label
      >
      <input
        id="nome"
        v-model="nome"
        type="text"
        class="mt-1 px-4 py-2 block w-full rounded-md bg-gray-200"
        placeholder="Sociedade Esportiva Palmeiras"
        required
        minlength="5"
      />
    </div>
    <div>
      <label
        for="sigla"
        class="block text-sm font-bold text-gray-700 uppercase"
        >Sigla</label
      >
      <input
        id="sigla"
        v-model="sigla"
        type="text"
        maxlength="3"
        class="mt-1 px-4 py-2 block w-full rounded-md bg-gray-200"
        placeholder="PAL"
        required
      />
    </div>
    <div>
      <label
        for="fundacao"
        class="block text-sm font-bold text-gray-700 uppercase"
        >Ano de Fundação</label
      >
      <input
        id="fundacao"
        v-model="fundacao"
        type="number"
        class="mt-1 px-4 py-2 block w-full rounded-md bg-gray-200"
        placeholder="1914"
        required
      />
    </div>
    <button
      type="submit"
      class="px-4 py-2 bg-black text-white rounded-md"
    >
      Registrar Time
    </button>
  </form>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'RegisterTeamForm',
  setup() {
    const nome = ref('');
    const sigla = ref('');
    const fundacao = ref('');

    const handleSubmit = async () => {
      const formData = {
        nome: nome.value,
        sigla: sigla.value,
        fundacao: fundacao.value,
      };

      try {
        const response = await fetch(
          'https://fut-api-441c9ac2d267.herokuapp.com/api/times',
          {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json',
            },
            body: JSON.stringify(formData),
          }
        );

        if (response.ok) {
          const result = await response.json();
          console.log('Team Registered Successfully:', result);
          console.log(result);
          nome.value = '';
          sigla.value = '';
          fundacao.value = '';
        } else {
          const error = await response.json();
          console.error('Error Registering Team:', error);
        }
      } catch (error) {
        console.error('Request Failed:', error);
      }
    };

    return {
      nome,
      sigla,
      fundacao,
      handleSubmit,
    };
  },
});
</script>
