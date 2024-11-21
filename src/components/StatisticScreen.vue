<template>
  <div class="statistics-screen p-4">
    <h2 class="text-2xl font-semibold mb-4">Estatísticas</h2>

    <div class="bg-white rounded-lg shadow-sm p-4 mb-6">
      <h3 class="text-lg font-semibold mb-4">Clube</h3>
      <p class="text-lg font-medium">Quantidade Total de Times: {{ totalTeams }}</p>
      <p class="text-lg font-medium">Clube Mais Antigo: {{ clubOldest }}</p>
      <p class="text-lg font-medium">Clube Mais Novo: {{ clubNewest }}</p>
      <p class="text-lg font-medium">Clube Com Mais Jogadores: {{ clubMostPlayers }}</p>
      <p class="text-lg font-medium">Clube Com Menos Jogadores: {{ clubFewestPlayers }}</p>
    </div>

    <div class="bg-white rounded-lg shadow-sm p-4">
      <h3 class="text-lg font-semibold mb-4">Jogador</h3>
      <p class="text-lg font-medium">Quantidade Total de Jogadores: {{ totalPlayers }}</p>
      <p class="text-lg font-medium">Jogador Mais Velho: {{ oldestPlayer }}</p>
      <p class="text-lg font-medium">Jogador Mais Novo: {{ youngestPlayer }}</p>
      <p class="text-lg font-medium">Quantidade Jogadores Sem Clube: {{ playersWithoutTeamCount }}</p>
      <p class="text-lg font-medium">Posição Mais Adicionada: {{ mostCommonPosition }}</p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue';

interface Player {
  nome: string;
  time?: string;
  posicao: string;
  idade: number;
}

interface Team {
  nome: string;
  sigla: string;
  fundacao: number;
  jogadores: Player[];
}

export default defineComponent({
  name: 'StatisticsScreen',
  setup() {
    const teams = ref<Team[]>([]);
    const playersWithoutTeam = ref<Player[]>([]);

    const totalTeams = ref(0);
    const totalPlayers = ref(0);
    const clubOldest = ref('');
    const clubNewest = ref('');
    const clubMostPlayers = ref('');
    const clubFewestPlayers = ref('');
    const oldestPlayer = ref('');
    const youngestPlayer = ref('');
    const playersWithoutTeamCount = ref(0);
    const mostCommonPosition = ref('');

    const fetchStatistics = async () => {
      try {
        const teamResponse = await fetch('https://fut-api-441c9ac2d267.herokuapp.com/api/times');
        if (!teamResponse.ok) {
          throw new Error('Failed to fetch teams');
        }
        const teamData: Team[] = await teamResponse.json();

        const playerResponse = await fetch('https://fut-api-441c9ac2d267.herokuapp.com/api/jogadores');
        if (!playerResponse.ok) {
          throw new Error('Failed to fetch players');
        }
        const playerData: Player[] = await playerResponse.json();

        teams.value = teamData.map((team) => ({
          ...team,
          jogadores: playerData.filter((player) => player.time === team.sigla),
        }));

        playersWithoutTeam.value = playerData.filter((player) => !player.time);

        totalTeams.value = teams.value.length;
        totalPlayers.value = playerData.length;

        const oldest = teams.value.reduce((oldest, team) =>
          team.fundacao < oldest.fundacao ? team : oldest
        );
        clubOldest.value = teams.value
          .filter(team => team.fundacao === oldest.fundacao)
          .map(team => `${team.nome} (${team.fundacao})`)
          .join(' | ');

        const newest = teams.value.reduce((newest, team) =>
          team.fundacao > newest.fundacao ? team : newest
        );
        clubNewest.value = teams.value
          .filter(team => team.fundacao === newest.fundacao)
          .map(team => `${team.nome} (${team.fundacao})`)
          .join(' | ');

        const mostPlayers = teams.value.reduce((most, team) =>
          team.jogadores.length > most.jogadores.length ? team : most
        );
        clubMostPlayers.value = teams.value
          .filter(team => team.jogadores.length === mostPlayers.jogadores.length)
          .map(team => `${team.nome} (${team.jogadores.length})`)
          .join(' | ');

        const fewestPlayers = teams.value.reduce((fewest, team) =>
          team.jogadores.length < fewest.jogadores.length ? team : fewest
        );
        clubFewestPlayers.value = teams.value
          .filter(team => team.jogadores.length === fewestPlayers.jogadores.length)
          .map(team => `${team.nome} (${team.jogadores.length})`)
          .join(' | ');

        const oldestPlayerData = playerData.reduce((oldest, player) =>
          player.idade > oldest.idade ? player : oldest
        );
        oldestPlayer.value = `${oldestPlayerData.nome} (${oldestPlayerData.idade} anos)`;

        const youngestPlayerData = playerData.reduce((youngest, player) =>
          player.idade < youngest.idade ? player : youngest
        );
        youngestPlayer.value = `${youngestPlayerData.nome} (${youngestPlayerData.idade} anos)`;

        playersWithoutTeamCount.value = playersWithoutTeam.value.length;

        const positionCounts = playerData.reduce((counts, player) => {
          counts[player.posicao] = (counts[player.posicao] || 0) + 1;
          return counts;
        }, {} as { [key: string]: number });
        const mostCommon = Object.entries(positionCounts).reduce((most, [position, count]) =>
          count > most.count ? { position, count } : most
        , { position: '', count: 0 });
        mostCommonPosition.value = `${mostCommon.position} (${mostCommon.count})`;

      } catch (error) {
        console.error('Error fetching statistics:', error);
      }
    };

    onMounted(fetchStatistics);

    return {
      totalTeams,
      totalPlayers,
      clubOldest,
      clubNewest,
      clubMostPlayers,
      clubFewestPlayers,
      oldestPlayer,
      youngestPlayer,
      playersWithoutTeamCount,
      mostCommonPosition,
    };
  },
});</script>