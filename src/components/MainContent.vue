<template>
  <div class="p-6">
    <component :is="currentComponent"></component>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';

export default defineComponent({
  name: 'MainContent',
  props: {
    currentComponent: {
      type: Object as PropType<any>,
      required: true,
    },
  },
});
</script>
