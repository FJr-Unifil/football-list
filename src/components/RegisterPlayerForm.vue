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
      >
        <option value="GOL" selected="true">GOL</option>
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
    const posicao = ref('GOL');

    const handleSubmit = () => {
      console.log('Player Registered:', {
        nome: nome.value,
        idade: idade.value,
        posicao: posicao.value,
      });
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
