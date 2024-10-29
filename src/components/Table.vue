<template>
  <div v-if="teams.length === 0" class="text-center text-gray-500">
    Empty! Add a club first
  </div>
  <table v-else class="w-full border-collapse border border-gray-200">
    <thead>
      <tr class="bg-gray-100">
        <th class="border p-2">Name</th>
        <th class="border p-2">Short Name</th>
        <th class="border p-2">Favorite Player</th>
        <th class="border p-2">Actions</th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="(team, index) in teams"
        :key="index"
        class="text-center"
      >
        <td class="border p-2">{{ team.name }}</td>
        <td class="border p-2">{{ team.shortName }}</td>
        <td class="border p-2">{{ team.favoritePlayer }}</td>
        <td class="border p-2">
          <ButtonComponent
            colorClass="bg-red-500 text-white"
            @click="removeTeam(index)"
            >Delete</ButtonComponent
          >
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';
import ButtonComponent from './Button.vue';

export default defineComponent({
  components: { ButtonComponent },
  props: {
    teams: {
      type: Array as PropType<
        Array<{
          name: string;
          shortName: string;
          favoritePlayer: string;
        }>
      >,
      required: true,
    },
  },
  emits: ['remove'],
  methods: {
    removeTeam(index: number) {
      this.$emit('remove', index);
    },
  },
});
</script>
