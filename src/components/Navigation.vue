<template>
  <nav :class="[
    'fixed left-0 top-0 h-screen bg-white dark:bg-gray-800 shadow-lg flex flex-col transition-all duration-300',
    isCollapsed ? 'w-20' : 'w-64'
  ]">
    <!-- Toggle Button -->
    <button 
      @click="toggleCollapse" 
      class="absolute -right-3 top-6 bg-white dark:bg-gray-800 p-1.5 rounded-full shadow-lg border border-gray-200 dark:border-gray-700"
    >
      <svg 
        xmlns="http://www.w3.org/2000/svg" 
        class="h-4 w-4 text-gray-600 dark:text-gray-300 transition-transform duration-300"
        :class="{ 'rotate-180': isCollapsed }"
        fill="none" 
        viewBox="0 0 24 24" 
        stroke="currentColor"
      >
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
      </svg>
    </button>

    <!-- Logo Section -->
    <div class="p-6 border-b border-gray-200 dark:border-gray-700">
      <router-link to="/" class="text-2xl font-bold flex items-center" style="color: var(--primary-color)">
        <span class="text-2xl">P</span>
        <span :class="['transition-opacity duration-300', isCollapsed ? 'opacity-0 w-0' : 'opacity-100']">ortfolio</span>
      </router-link>
    </div>

    <!-- Navigation Links -->
    <div class="flex-1 py-6 px-4">
      <div class="flex flex-col gap-4">
        <router-link 
          v-for="(item, index) in navItems" 
          :key="index"
          :to="item.to" 
          class="nav-link flex items-center gap-3 p-3 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700 transition-colors group"
          :class="{ 'bg-gray-100 dark:bg-gray-700': $route.path === item.to }"
        >
          <div class="min-w-[20px]" v-html="item.icon"></div>
          <span :class="['transition-opacity duration-300', isCollapsed ? 'opacity-0 w-0' : 'opacity-100']">
            {{ item.text }}
          </span>
          <div 
            v-if="isCollapsed" 
            class="absolute left-full ml-2 px-2 py-1 bg-gray-800 text-white text-sm rounded opacity-0 group-hover:opacity-100 transition-opacity pointer-events-none"
          >
            {{ item.text }}
          </div>
        </router-link>
      </div>
    </div>

    <!-- Bottom Section with Dark Mode Toggle -->
    <div class="p-6 border-t border-gray-200 dark:border-gray-700">
      <button 
        @click="toggleDarkMode" 
        class="w-full flex items-center justify-center gap-3 p-3 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700 transition-colors group"
      >
        <svg v-if="isDark" xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-yellow-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z" />
        </svg>
        <svg v-else xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z" />
        </svg>
        <span :class="['transition-opacity duration-300', isCollapsed ? 'opacity-0 w-0' : 'opacity-100']">
          {{ isDark ? 'Light Mode' : 'Dark Mode' }}
        </span>
        <div 
          v-if="isCollapsed" 
          class="absolute left-full ml-2 px-2 py-1 bg-gray-800 text-white text-sm rounded opacity-0 group-hover:opacity-100 transition-opacity pointer-events-none"
        >
          {{ isDark ? 'Light Mode' : 'Dark Mode' }}
        </div>
      </button>
    </div>
  </nav>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const props = defineProps({
  isDark: {
    type: Boolean,
    required: true
  }
});

const emit = defineEmits(['toggle-dark-mode']);
const isCollapsed = ref(false);

const toggleDarkMode = () => {
  emit('toggle-dark-mode');
};

const toggleCollapse = () => {
  isCollapsed.value = !isCollapsed.value;
  localStorage.setItem('sidebarCollapsed', isCollapsed.value);
};

const navItems = [
  {
    to: '/',
    text: 'Home',
    icon: `<svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6" />
          </svg>`
  },
  {
    to: '/blog',
    text: 'Blog',
    icon: `<svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 20H5a2 2 0 01-2-2V6a2 2 0 012-2h10a2 2 0 012 2v1m2 13a2 2 0 01-2-2V7m2 13a2 2 0 002-2V9.5a2.5 2.5 0 00-2.5-2.5H15" />
          </svg>`
  },
  {
    to: '/portfolio',
    text: 'Portfolio',
    icon: `<svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 13h6m-3-3v6m5 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z" />
          </svg>`
  },
  {
    to: '/projects',
    text: 'Projects',
    icon: `<svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2V6zM14 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2V6zM4 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2v-2zM14 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2v-2z" />
          </svg>`
  }
];

// Load collapsed state on mount
onMounted(() => {
  isCollapsed.value = localStorage.getItem('sidebarCollapsed') === 'true';
});
</script>

<style scoped>
.nav-link {
  @apply text-gray-700 dark:text-gray-200 transition-colors duration-200;
}

.nav-link:hover {
  @apply text-gray-900 dark:text-white;
}

.nav-link.router-link-active {
  @apply font-medium;
  color: var(--primary-color);
}
</style> 