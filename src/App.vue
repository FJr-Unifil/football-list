<template>
  <div class="min-h-screen bg-gray-50 p-4">
    <div class="max-w-6xl mx-auto space-y-6">
      <div class="bg-white rounded-xl shadow-sm p-6">
        <NavigationBar
          :screens="screens"
          :currentScreen="currentScreen"
          @update:currentScreen="updateCurrentScreen"
        />
      </div>

      <div class="bg-white rounded-xl shadow-sm">
        <MainContent :currentComponent="currentComponent" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, computed } from 'vue';
import { PlusCircle, Users, Home, ChartArea } from 'lucide-vue-next';
import NavigationBar from './components/NavigationBar.vue';
import MainContent from './components/MainContent.vue';
import RegisterScreen from './components/RegisterScreen.vue';
import ListScreen from './components/ListScreen.vue';
import StatisticsScreen from './components/StatisticScreen.vue';

export default defineComponent({
  name: 'App',
  components: {
    NavigationBar,
    MainContent,
    PlusCircle,
    Users,
    Home,
  },
  setup() {
    const currentScreen = ref<'register' | 'list' | 'statistics'>(
      'register'
    );
    const screens = [
      { id: 'register', name: 'REGISTRAR', icon: PlusCircle },
      { id: 'list', name: 'TIMES E JOGADORES', icon: Users },
      { id: 'statistics', name: 'ESTATÍSTICAS', icon: ChartArea },
    ];

    const currentComponent = computed(() => {
      const components = {
        register: RegisterScreen,
        list: ListScreen,
        statistics: StatisticsScreen,
      };
      return components[currentScreen.value];
    });

    const updateCurrentScreen = (
      screenId: 'register' | 'list' | 'statistics'
    ) => {
      currentScreen.value = screenId;
    };

    return {
      currentScreen,
      screens,
      currentComponent,
      updateCurrentScreen,
    };
  },
});
</script>
