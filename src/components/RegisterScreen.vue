<template>
  <div class="register-screen p-4">
    <h2 class="text-2xl font-semibold mb-4">
      Register Team and Players
    </h2>
    <form @submit.prevent="registerTeam">
      <div class="mb-4">
        <label
          for="teamName"
          class="block text-sm font-medium text-gray-700"
          >Team Name</label
        >
        <input
          v-model="teamName"
          type="text"
          id="teamName"
          class="mt-1 block w-full border-gray-300 rounded-md shadow-sm"
          placeholder="Enter team name"
          required
        />
      </div>
      <div class="mb-4">
        <label
          for="players"
          class="block text-sm font-medium text-gray-700"
          >Players</label
        >
        <textarea
          v-model="players"
          id="players"
          class="mt-1 block w-full border-gray-300 rounded-md shadow-sm"
          placeholder="Enter player names, separated by commas"
          required
        ></textarea>
      </div>
      <button
        type="submit"
        class="inline-flex items-center px-4 py-2 border border-transparent text-sm font-medium rounded-md shadow-sm text-white bg-primary hover:bg-primary-dark"
      >
        Register Team
      </button>
    </form>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'RegisterScreen',
  setup() {
    const teamName = ref('');
    const players = ref('');

    const registerTeam = () => {
      console.log('Team registered:', teamName.value);
      console.log('Players:', players.value);
      // Clear fields
      teamName.value = '';
      players.value = '';
    };

    return {
      teamName,
      players,
      registerTeam,
    };
  },
});
</script>
