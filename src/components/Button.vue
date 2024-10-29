<template>
  <button :class="['px-4 py-2 rounded', colorClass]" @click="onClick">
    <slot />
  </button>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  props: {
    colorClass: {
      type: String,
      default: 'bg-blue-500 text-white',
    },
  },
  emits: ['click'],
  methods: {
    onClick() {
      this.$emit('click');
    },
  },
});
</script>
