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
        placeholder="Rony, o Rústico"
        minlength="5"
        required
      />
    </div>
    <div>
      <label
        for="posicao"
        class="block text-sm font-bold text-gray-700 uppercase"
        >Posição</label
      >
      <select
        v-model="posicao"
        name="posicao"
        id="posicao"
        class="mt-1 px-4 py-2 block w-full rounded-md bg-gray-200"
        required
      >
        <option value="" disabled selected>
          Qual a posição do jogador? 🏳️‍🌈🫵
        </option>
        <option value="GOL">GOL</option>
        <option value="ZAG">ZAG</option>
        <option value="LD">LD</option>
        <option value="LE">LE</option>
        <option value="ADE">ADE</option>
        <option value="ADD">ADD</option>
        <option value="VOL">VOL</option>
        <option value="MC">MC</option>
        <option value="MEI">MEI</option>
        <option value="ME">ME</option>
        <option value="MD">MD</option>
        <option value="PE">PE</option>
        <option value="SA">SA</option>
        <option value="ATA">ATA</option>
        <option value="PD">PD</option>
      </select>
    </div>
    <div>
      <label
        for="idade"
        class="block text-sm font-bold text-gray-700 uppercase"
        >Idade</label
      >
      <input
        id="idade"
        v-model="idade"
        type="number"
        class="mt-1 px-4 py-2 block w-full rounded-md bg-gray-200"
        placeholder="29"
        required
      />
    </div>
    <button
      type="submit"
      class="px-4 py-2 bg-black text-white rounded-md uppercase"
    >
      Registrar Jogador
    </button>
  </form>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'RegisterPlayerForm',
  setup() {
    const nome = ref('');
    const idade = ref('');
    const posicao = ref('');

    const handleSubmit = async () => {
      const formData = {
        nome: nome.value,
        idade: idade.value,
        posicao: posicao.value,
      };

      try {
        const response = await fetch(
          'https://fut-api-441c9ac2d267.herokuapp.com/api/jogadores',
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
          console.log('Player Registered Successfully:', result);
          console.log(result);
          nome.value = '';
          idade.value = '';
          posicao.value = '';
        } else {
          const error = await response.json();
          console.error('Error Registering Player:', error);
        }
      } catch (error) {
        console.error('Request Failed:', error);
      }
    };

    return {
      nome,
      idade,
      posicao,
      handleSubmit,
    };
  },
});
</script>
