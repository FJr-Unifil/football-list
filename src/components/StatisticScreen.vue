<template>
  <div class="statistics-screen p-4">
    <h2 class="text-2xl font-semibold mb-4">Statistics</h2>
    <div class="bg-white rounded-lg shadow-sm p-4">
      <p class="text-lg font-medium">Total Teams: {{ totalTeams }}</p>
      <p class="text-lg font-medium">
        Total Players: {{ totalPlayers }}
      </p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, computed } from 'vue';

export default defineComponent({
  name: 'StatisticsScreen',
  setup() {
    const teams = [
      {
        name: 'Team A',
        players: ['Player 1', 'Player 2', 'Player 3'],
      },
      {
        name: 'Team B',
        players: ['Player 4', 'Player 5', 'Player 6'],
      },
    ];

    const totalTeams = computed(() => teams.length);
    const totalPlayers = computed(() =>
      teams.reduce((count, team) => count + team.players.length, 0)
    );

    return {
      totalTeams,
      totalPlayers,
    };
  },
});
</script>
