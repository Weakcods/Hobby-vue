<template>
  <div :class="{ 'dark': isDark }">
    <div class="min-h-screen bg-gray-50 dark:bg-gray-900 transition-colors duration-200">
      <Navigation 
        :is-dark="isDark" 
        @toggle-dark-mode="toggleDarkMode"
        ref="navigationRef"
      />

      <div 
        class="transition-all duration-300"
        :class="[navigationRef?.isCollapsed ? 'ml-20' : 'ml-64']"
      >
        <main class="p-6">
          <router-view v-slot="{ Component }">
            <transition name="fade" mode="out-in">
              <component :is="Component" />
            </transition>
          </router-view>
        </main>

        <footer class="bg-gray-900 dark:bg-gray-800 text-white py-8 mt-16">
          <div class="container">
            <Footer />
          </div>
        </footer>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import Navigation from './components/Navigation.vue';

const isDark = ref(false);
const navigationRef = ref(null);

const toggleDarkMode = () => {
  isDark.value = !isDark.value;
  localStorage.setItem('darkMode', isDark.value);
  document.documentElement.classList.toggle('dark', isDark.value);
};

onMounted(() => {
  const savedDarkMode = localStorage.getItem('darkMode') === 'true';
  isDark.value = savedDarkMode;
  document.documentElement.classList.toggle('dark', savedDarkMode);
});
</script>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>