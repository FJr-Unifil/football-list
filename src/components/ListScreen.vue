<template>
  <div class="list-screen p-4">
    <h2 class="text-2xl font-semibold mb-4">Times e Jogadores</h2>
    <ul class="space-y-4">
      <li
        v-for="team in teams"
        :key="team.sigla"
        class="p-4 bg-white rounded-lg shadow-sm"
      >
        <h3 class="text-xl font-medium">{{ team.nome }}</h3>
        <ul class="ml-4 list-disc">
          <li v-for="player in team.players" :key="player.nome">
            {{ player.nome }}
          </li>
        </ul>
      </li>
    </ul>
    <div class="p-4 bg-white rounded-lg shadow-sm mt-6">
      <h3 class="text-xl font-medium">Sem Clube 🚫</h3>
      <ul class="ml-4 list-disc">
        <li v-for="player in playersWithoutTeam" :key="player.nome">
          {{ player.nome }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue';

interface Player {
  nome: string;
  siglaTime?: string;
}

interface Team {
  nome: string;
  sigla: string;
  players: Player[];
}

export default defineComponent({
  name: 'ListScreen',
  setup() {
    const teams = ref<Team[]>([]);
    const playersWithoutTeam = ref<Player[]>([]);

    const fetchTeamsAndPlayers = async () => {
      try {
        const teamResponse = await fetch('https://fut-api-441c9ac2d267.herokuapp.com/api/times');
        if (!teamResponse.ok) {
          throw new Error('Failed to fetch teams');
        }
        const teamData: Team[] = await teamResponse.json();
        teams.value = teamData.map((team) => ({
          ...team,
          players: [],
        }));

        const playerResponse = await fetch('https://fut-api-441c9ac2d267.herokuapp.com/api/jogadores');
        if (!playerResponse.ok) {
          throw new Error('Failed to fetch players');
        }
        const playerData: Player[] = await playerResponse.json();
        playersWithoutTeam.value = [];

        playerData.forEach((player) => {
          const team = teams.value.find(
            (team) => team.sigla === player.siglaTime
          );
          if (team) {
            team.players.push(player);
          } else {
            playersWithoutTeam.value.push(player);
          }
        });
      } catch (error) {
        console.error('Error fetching teams and players:', error);
      }
    };

    onMounted(fetchTeamsAndPlayers);

    return {
      teams,
      playersWithoutTeam,
    };
  },
  created() {
    this.fetchData();  // Chama a função para buscar os dados ao criar o componente
  },
  methods: {
    async fetchData() {
      try {
        const response = await axios.get('https://fut-api-441c9ac2d267.herokuapp.com/api/times');
        this.teams = response.data;  // Armazenando os dados recebidos na variável teams
      } catch (error) {
        console.error('Erro ao fazer a requisição:', error);
      }
    },
  },
});
</script>
