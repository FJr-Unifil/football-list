<template>
  <div class="max-w-2xl mx-auto p-4">
    <div class="flex space-x-4 mb-4">
      <InputComponent v-model="name" placeholder="Team Name" />
      <InputComponent v-model="shortName" placeholder="Short Name" />
      <InputComponent
        v-model="favoritePlayer"
        placeholder="Favorite Player"
      />
      <ButtonComponent
        colorClass="bg-blue-500 text-white"
        @click="addTeam"
        >Add Team</ButtonComponent
      >
    </div>
    <TableComponent :teams="teams" @remove="removeTeam" />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import InputComponent from './components/Input.vue';
import ButtonComponent from './components/Button.vue';
import TableComponent from './components/Table.vue';

interface Team {
  name: string;
  shortName: string;
  favoritePlayer: string;
}

export default defineComponent({
  components: {
    InputComponent,
    ButtonComponent,
    TableComponent,
  },
  setup() {
    const name = ref<string>('');
    const shortName = ref<string>('');
    const favoritePlayer = ref<string>('');
    const teams = ref<Team[]>([]);

    const addTeam = () => {
      if (name.value && shortName.value && favoritePlayer.value) {
        teams.value.push({
          name: name.value,
          shortName: shortName.value,
          favoritePlayer: favoritePlayer.value,
        });
        name.value = '';
        shortName.value = '';
        favoritePlayer.value = '';
      }
    };

    const removeTeam = (index: number) => {
      teams.value.splice(index, 1);
    };

    return {
      name,
      shortName,
      favoritePlayer,
      teams,
      addTeam,
      removeTeam,
    };
  },
});
</script>
