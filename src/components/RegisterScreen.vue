<script lang="ts">
import { defineComponent, ref } from 'vue';
import RegisterPlayerForm from './RegisterPlayerForm.vue';
import RegisterTeamForm from './RegisterTeamForm.vue';

export default defineComponent({
  name: 'RegisterScreen',
  components: {
    RegisterPlayerForm,
    RegisterTeamForm,
  },
  setup() {
    const currentTab = ref<'player' | 'team'>('player');
    const tabs: Array<{ id: 'player' | 'team'; name: string }> = [
      { id: 'player', name: 'REGISTRAR JOGADOR' },
      { id: 'team', name: 'REGISTRAR TIME' },
    ];

    return {
      currentTab,
      tabs,
    };
  },
});
</script>

<template>
  <div class="space-y-6">
    <div class="flex space-x-4 border-b pb-2">
      <button
        v-for="tab in tabs"
        :key="tab.id"
        @click="currentTab = tab.id"
        class="px-4 py-2 text-sm font-medium transition-colors"
        :class="
          currentTab === tab.id
            ? 'border-b-2 border-black text-black'
            : 'text-gray-500 hover:text-black'
        "
      >
        {{ tab.name }}
      </button>
    </div>

    <div>
      <RegisterPlayerForm v-if="currentTab === 'player'" />
      <RegisterTeamForm v-else-if="currentTab === 'team'" />
    </div>
  </div>
</template>
