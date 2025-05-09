<template>
  <div id="app" class="min-h-screen bg-gray-50 dark:bg-gray-900">
    <!-- Enhanced Sidebar Component -->
    <Sidebar />
    
    <!-- Main Content with Proper Spacing -->
    <div 
      class="transition-all duration-300" 
      :class="{ 'md:ml-72': true }"
    >
      <div class="container mx-auto px-4 py-12">
        <!-- Dynamic content based on active tab -->
        <component :is="currentComponent" />
      </div>
    </div>
    
    <!-- Notification Toast -->
    <transition name="toast">
      <div 
        v-if="notification.show" 
        class="fixed bottom-4 right-4 bg-gray-800 text-white px-4 py-2 rounded-lg shadow-lg z-50 flex items-center"
      >
        <span>{{ notification.message }}</span>
        <button @click="hideNotification" class="ml-3 text-gray-300 hover:text-white">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>
      </div>
    </transition>
    
    <!-- Overlay for mobile when sidebar is open -->
    <div 
      v-if="isSidebarOpen" 
      class="md:hidden fixed inset-0 bg-black bg-opacity-50 z-10"
      @click="closeSidebar"
    ></div>
  </div>
</template>

<script>
import ColorGenerator from './components/HomePage.vue'
import Sidebar from './components/Sidebar.vue'

export default {
  name: 'App',
  components: {
    ColorGenerator,
    Sidebar
  },
  data() {
    return {
      isSidebarOpen: false,
      activeTab: 'generator',
      notification: {
        show: false,
        message: '',
        timeout: null
      }
    }
  },
  computed: {
    currentComponent() {
      // Return the appropriate component based on active tab
      switch(this.activeTab) {
        case 'generator':
          return ColorGenerator;
        case 'saved':
          // You would create and import these components
          // return SavedPalettes;
          return ColorGenerator; // Fallback for now
        case 'settings':
          // return Settings;
          return ColorGenerator; // Fallback for now
        default:
          return ColorGenerator;
      }
    }
  },
  mounted() {
    // Listen for sidebar toggle events
    this.$root.$on('sidebar-toggled', this.handleSidebarToggle);
    
    // Listen for tab change events
    this.$root.$on('change-tab', this.handleTabChange);
    
    // Listen for notification events
    this.$root.$on('show-notification', this.showNotification);
  },
  beforeDestroy() {
    // Clean up event listeners
    this.$root.$off('sidebar-toggled', this.handleSidebarToggle);
    this.$root.$off('change-tab', this.handleTabChange);
    this.$root.$off('show-notification', this.showNotification);
    
    // Clear any pending notification timeouts
    if (this.notification.timeout) {
      clearTimeout(this.notification.timeout);
    }
  },
  methods: {
    handleSidebarToggle(isOpen) {
      this.isSidebarOpen = isOpen;
    },
    closeSidebar() {
      this.isSidebarOpen = false;
      this.$root.$emit('close-sidebar');
    },
    handleTabChange(tab) {
      this.activeTab = tab;
    },
    showNotification(message) {
      // Clear any existing timeout
      if (this.notification.timeout) {
        clearTimeout(this.notification.timeout);
      }
      
      // Show new notification
      this.notification.message = message;
      this.notification.show = true;
      
      // Auto-hide after 3 seconds
      this.notification.timeout = setTimeout(() => {
        this.hideNotification();
      }, 3000);
    },
    hideNotification() {
      this.notification.show = false;
    }
  }
}
</script>

<style>
/* Import General Sans font */
@font-face {
  font-family: 'General Sans';
  src: url('./assets/fonts/GeneralSans-Regular.otf') format('opentype');
  font-weight: 400;
  font-style: normal;
  font-display: swap;
}

@font-face {
  font-family: 'General Sans';
  src: url('./assets/fonts/GeneralSans-Bold.otf') format('opentype');
  font-weight: 700;
  font-style: normal;
  font-display: swap;
}

/* Apply the font globally */
body {
  font-family: 'General Sans', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* Tailwind dark mode */
.dark {
  color-scheme: dark;
}

/* Transition for theme changes */
* {
  transition-property: color, background-color, border-color, fill;
  transition-duration: 200ms;
}

/* Toast animation */
.toast-enter-active,
.toast-leave-active {
  transition: all 0.3s ease;
}

.toast-enter-from,
.toast-leave-to {
  opacity: 0;
  transform: translateY(20px);
}
</style>
