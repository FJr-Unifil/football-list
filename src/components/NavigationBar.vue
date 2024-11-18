<template>
  <nav class="flex space-x-4">
    <button
      v-for="screen in screens"
      :key="screen.id"
      class="inline-flex items-center px-4 py-2 rounded-md text-sm font-medium transition-colors"
      :class="
        currentScreen === screen.id
          ? 'bg-black text-white'
          : 'text-gray-600 hover:bg-gray-100'
      "
      @click="$emit('update:currentScreen', screen.id)"
    >
      <component :is="screen.icon" class="w-4 h-4 mr-2" />
      {{ screen.name }}
    </button>
  </nav>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';

export default defineComponent({
  name: 'NavigationBar',
  props: {
    screens: {
      type: Array as PropType<
        Array<{ id: string; name: string; icon: any }>
      >,
      required: true,
    },
    currentScreen: {
      type: String,
      required: true,
    },
  },
});
</script>
