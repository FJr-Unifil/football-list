<template>
  <div class="list-screen p-4">
    <h2 class="text-2xl font-semibold mb-4">Times e Jogadores</h2>
    <ul class="space-y-4">
      <li
        v-for="team in teams"
        :key="team.sigla"
        class="p-4 bg-white rounded-lg shadow-sm"
      >
        <h3 class="text-xl font-medium flex items-center justify-between">
          {{ team.nome }}
          <div class="flex space-x-2">
            <button
              class="bg-blue-500 text-white px-2 py-1 rounded"
              @click="openAddPlayerPopup(team)"
            >
              Adicionar Jogador
            </button>
            <button
              v-if="team.jogadores.length > 0"
              class="bg-red-500 text-white px-2 py-1 rounded"
              @click="openRemovePlayerPopup(team)"
            >
              Remover Jogador
            </button>
          </div>
        </h3>
        <ul class="ml-4 list-disc">
          <li v-if="team.jogadores.length === 0" class="text-gray-500">
            No momento, o clube NÃO possui nenhum jogador cadastrado ❌
          </li>
          <li v-else v-for="player in team.jogadores" :key="player.nome">
            {{ player.nome }}
          </li>
        </ul>
      </li>
    </ul>
    <div class="p-4 bg-white rounded-lg shadow-sm mt-6">
      <h3 class="text-xl font-medium">Sem Clube 🚫</h3>
      <ul class="ml-4 list-disc">
        <li v-if="playersWithoutTeam.length === 0" class="text-gray-500">
          Não há jogadores sem clube no momento.
        </li>
        <li v-else v-for="player in playersWithoutTeam" :key="player.nome">
          {{ player.nome }}
        </li>
      </ul>
    </div>

    <div
      v-if="isAddPopupOpen"
      class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center"
    >
      <div class="bg-white p-6 rounded shadow-lg w-96">
        <h3 class="text-lg font-semibold mb-4">
          Adicionar jogador ao {{ selectedTeam?.nome }}
        </h3>
        <div class="mb-4">
          <label class="block font-medium mb-2">Selecione um jogador:</label>
          <select v-model="selectedPlayer" class="w-full border rounded px-2 py-1">
            <option
              v-for="player in playersWithoutTeam"
              :key="player.nome"
              :value="player"
            >
              {{ player.nome }} - {{ player.posicao }} - {{ player.idade }} anos
            </option>
          </select>
        </div>
        <div class="flex justify-end">
          <button class="bg-gray-500 text-white px-3 py-1 rounded mr-2" @click="closePopup">
            Cancelar
          </button>
          <button class="bg-green-500 text-white px-3 py-1 rounded" @click="addPlayerToTeam">
            Adicionar
          </button>
        </div>
      </div>
    </div>

    <div
      v-if="isRemovePopupOpen"
      class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center"
    >
      <div class="bg-white p-6 rounded shadow-lg w-96">
        <h3 class="text-lg font-semibold mb-4">
          Remover jogador do {{ selectedTeam?.nome }}
        </h3>
        <div class="mb-4">
          <label class="block font-medium mb-2">Selecione um jogador:</label>
          <select v-model="selectedPlayer" class="w-full border rounded px-2 py-1">
            <option
              v-for="player in selectedTeam?.jogadores"
              :key="player.nome"
              :value="player"
            >
              {{ player.nome }}
            </option>
          </select>
        </div>
        <div class="flex justify-end">
          <button class="bg-gray-500 text-white px-3 py-1 rounded mr-2" @click="closePopup">
            Cancelar
          </button>
          <button class="bg-red-500 text-white px-3 py-1 rounded" @click="removePlayerFromTeam">
            Remover
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

interface Player {
  nome: string;
  time?: string;
  posicao: string;
  idade: number;
}

interface Team {
  nome: string;
  sigla: string;
  jogadores: Player[];
}

export default defineComponent({
  name: 'ListScreen',
  setup() {
    const teams = ref<Team[]>([]);
    const playersWithoutTeam = ref<Player[]>([]);
    const isAddPopupOpen = ref(false);
    const isRemovePopupOpen = ref(false);
    const selectedTeam = ref<Team | null>(null);
    const selectedPlayer = ref<Player | null>(null);

    const openAddPlayerPopup = (team: Team) => {
      selectedTeam.value = team;
      isAddPopupOpen.value = true;
    };

    const openRemovePlayerPopup = (team: Team) => {
      selectedTeam.value = team;
      isRemovePopupOpen.value = true;
    };

    const closePopup = () => {
      isAddPopupOpen.value = false;
      isRemovePopupOpen.value = false;
      selectedPlayer.value = null;
    };

    const addPlayerToTeam = async () => {
      if (!selectedPlayer.value || !selectedTeam.value) return;

      try {
        const response = await fetch(
          `https://fut-api-441c9ac2d267.herokuapp.com/api/times/${selectedPlayer.value.nome}/${selectedTeam.value.sigla}`,
          { method: 'PUT' }
        );

        if (!response.ok) {
          throw new Error('Failed to add player to team');
        }

        selectedTeam.value.jogadores.push(selectedPlayer.value);
        playersWithoutTeam.value = playersWithoutTeam.value.filter(
          (player) => player.nome !== selectedPlayer.value?.nome
        );

        closePopup();
      } catch (error) {
        console.error('Error adding player to team:', error);
      }
    };

    const removePlayerFromTeam = async () => {
      if (!selectedPlayer.value || !selectedTeam.value) return;

      try {
        const response = await fetch(
          `https://fut-api-441c9ac2d267.herokuapp.com/api/times/${selectedPlayer.value.nome}/${selectedTeam.value.sigla}`,
          { method: 'DELETE' }
        );

        if (!response.ok) {
          throw new Error('Failed to remove player from team');
        }

        selectedTeam.value.jogadores = selectedTeam.value.jogadores.filter(
          (player) => player.nome !== selectedPlayer.value?.nome
        );
        playersWithoutTeam.value.push(selectedPlayer.value);

        closePopup();
      } catch (error) {
        console.error('Error removing player from team:', error);
      }
    };

    const fetchTeamsAndPlayers = async () => {
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
      } catch (error) {
        console.error('Error fetching teams and players:', error);
      }
    };

    fetchTeamsAndPlayers();

    return {
      teams,
      playersWithoutTeam,
      isAddPopupOpen,
      isRemovePopupOpen,
      selectedTeam,
      selectedPlayer,
      openAddPlayerPopup,
      openRemovePlayerPopup,
      closePopup,
      addPlayerToTeam,
      removePlayerFromTeam,
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