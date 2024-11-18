<template>
  <div class="list-screen p-4">
    <h2 class="text-2xl font-semibold mb-4">Teams & Players List</h2>
    <ul class="space-y-4">
      <li
        v-for="team in teams"
        :key="team.name"
        class="p-4 bg-white rounded-lg shadow-sm"
      >
        <h3 class="text-xl font-medium">{{ team.name }}</h3>
        <ul class="ml-4 list-disc">
          <li v-for="player in team.players" :key="player">
            {{ player }}
          </li>
        </ul>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'ListScreen',
  setup() {
    const teams = ref([
      {
        name: 'Team A',
        players: ['Player 1', 'Player 2', 'Player 3'],
      },
      {
        name: 'Team B',
        players: ['Player 4', 'Player 5', 'Player 6'],
      },
    ]);

    return {
      teams,
    };
  },
});
</script>
