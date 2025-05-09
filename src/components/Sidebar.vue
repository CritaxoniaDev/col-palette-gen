<template>
  <div>
    <!-- Main Sidebar -->
    <div 
      class="sidebar bg-white dark:bg-gray-800 shadow-xl h-screen fixed left-0 top-0 w-72 transition-all duration-300 z-20 border-r border-gray-100 dark:border-gray-700 overflow-hidden"
      :class="{ '-translate-x-full': !isOpen, 'md:translate-x-0': true }"
    >
      <!-- Logo and App Title with Animation -->
      <div class="p-6 border-b border-gray-200 dark:border-gray-700 relative overflow-hidden">
        <div class="absolute inset-0 bg-gradient-to-r from-indigo-50 to-transparent dark:from-indigo-900/20 dark:to-transparent opacity-50"></div>
        <div class="relative">
          <div class="flex items-center justify-center mb-6 group cursor-pointer" @click="triggerEasterEgg">
            <div class="relative">
              <svg width="48" height="48" viewBox="0 0 120 120" xmlns="http://www.w3.org/2000/svg" class="mr-3 transform transition-transform group-hover:rotate-12">
                <!-- Color wheel background with subtle shadow -->
                <filter id="shadow" x="-20%" y="-20%" width="140%" height="140%">
                  <feDropShadow dx="0" dy="2" stdDeviation="3" flood-opacity="0.3" flood-color="currentColor"/>
                </filter>
                <circle cx="60" cy="60" r="50" fill="#f8f9fa" class="dark:fill-gray-700" filter="url(#shadow)" />
                
                <!-- Animated color swatches -->
                <g class="color-swatches">
                  <rect x="30" y="35" width="25" height="25" rx="4" fill="#FF595E" class="swatch swatch-1" />
                  <rect x="65" y="35" width="25" height="25" rx="4" fill="#FFCA3A" class="swatch swatch-2" />
                  <rect x="30" y="65" width="25" height="25" rx="4" fill="#8AC926" class="swatch swatch-3" />
                  <rect x="65" y="65" width="25" height="25" rx="4" fill="#1982C4" class="swatch swatch-4" />
                </g>
                
                <!-- Center circle with pulse effect -->
                <circle cx="60" cy="60" r="12" fill="#6A4C93" class="center-circle" />
              </svg>
              
              <!-- Sparkle effect on hover -->
              <div class="absolute -top-1 -right-1 opacity-0 group-hover:opacity-100 transition-opacity duration-300">
                <svg width="20" height="20" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg" class="animate-pulse">
                  <path d="M12 2L15.09 8.26L22 9.27L17 14.14L18.18 21.02L12 17.77L5.82 21.02L7 14.14L2 9.27L8.91 8.26L12 2Z" fill="#FFD700" />
                </svg>
              </div>
            </div>
            <div>
              <h1 class="text-xl font-bold text-gray-800 dark:text-white bg-gradient-to-r from-indigo-500 via-purple-500 to-pink-500 bg-clip-text text-transparent">Palette Gen</h1>
              <div class="h-0.5 w-0 group-hover:w-full bg-gradient-to-r from-indigo-500 via-purple-500 to-pink-500 transition-all duration-300"></div>
            </div>
          </div>
          <p class="text-sm text-gray-600 dark:text-gray-400 text-center">Create beautiful color combinations</p>
        </div>
      </div>
      
      <!-- Navigation Links with Hover Effects -->
      <nav class="p-4">
        <ul class="space-y-2">
          <li>
            <a 
              href="#" 
              class="flex items-center p-3 rounded-lg transition-all duration-200 relative overflow-hidden group"
              :class="activeTab === 'generator' ? 'text-white' : 'text-gray-700 dark:text-gray-200 hover:bg-gray-100 dark:hover:bg-gray-700'"
              @click.prevent="setActiveTab('generator')"
            >
              <!-- Active background with animation -->
              <div 
                v-if="activeTab === 'generator'" 
                class="absolute inset-0 bg-gradient-to-r from-indigo-600 to-indigo-500 rounded-lg"
              ></div>
              
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-3 relative z-10" :class="activeTab === 'generator' ? 'text-white' : 'text-indigo-500'" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2V6zM14 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2V6zM4 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2v-2zM14 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2v-2z" />
              </svg>
              <span class="font-medium relative z-10">Generator</span>
              
              <!-- Right arrow indicator for active item -->
              <svg 
                v-if="activeTab === 'generator'" 
                xmlns="http://www.w3.org/2000/svg" 
                class="h-5 w-5 ml-auto relative z-10 text-white" 
                fill="none" 
                viewBox="0 0 24 24" 
                stroke="currentColor"
              >
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
              </svg>
            </a>
          </li>
          <li>
            <a 
              href="#" 
              class="flex items-center p-3 rounded-lg transition-all duration-200 relative overflow-hidden group"
              :class="activeTab === 'saved' ? 'text-white' : 'text-gray-700 dark:text-gray-200 hover:bg-gray-100 dark:hover:bg-gray-700'"
              @click.prevent="setActiveTab('saved')"
            >
              <!-- Active background with animation -->
              <div 
                v-if="activeTab === 'saved'" 
                class="absolute inset-0 bg-gradient-to-r from-indigo-600 to-indigo-500 rounded-lg"
              ></div>
              
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-3 relative z-10 transition-colors duration-200" :class="activeTab === 'saved' ? 'text-white' : 'text-gray-500 group-hover:text-indigo-500'" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 3v4M3 5h4M6 17v4m-2-2h4m5-16l2.286 6.857L21 12l-5.714 2.143L13 21l-2.286-6.857L5 12l5.714-2.143L13 3z" />
              </svg>
              <span class="font-medium relative z-10">Saved Palettes</span>
              
              <!-- Badge for saved palettes -->
              <span 
                class="ml-auto bg-indigo-100 text-indigo-800 dark:bg-indigo-900 dark:text-indigo-200 text-xs font-medium px-2 py-0.5 rounded-full relative z-10"
                :class="{ 'invisible': activeTab === 'saved' }"
              >
                {{ savedPalettesCount }}
              </span>
              
              <!-- Right arrow indicator for active item -->
              <svg 
                v-if="activeTab === 'saved'" 
                xmlns="http://www.w3.org/2000/svg" 
                class="h-5 w-5 ml-auto relative z-10 text-white" 
                fill="none" 
                viewBox="0 0 24 24" 
                stroke="currentColor"
              >
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
              </svg>
            </a>
          </li>
          <li>
            <a 
              href="#" 
              class="flex items-center p-3 rounded-lg transition-all duration-200 relative overflow-hidden group"
              :class="activeTab === 'settings' ? 'text-white' : 'text-gray-700 dark:text-gray-200 hover:bg-gray-100 dark:hover:bg-gray-700'"
              @click.prevent="setActiveTab('settings')"
            >
              <!-- Active background with animation -->
              <div 
                v-if="activeTab === 'settings'" 
                class="absolute inset-0 bg-gradient-to-r from-indigo-600 to-indigo-500 rounded-lg"
              ></div>
              
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-3 relative z-10 transition-colors duration-200" :class="activeTab === 'settings' ? 'text-white' : 'text-gray-500 group-hover:text-indigo-500'" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6V4m0 2a2 2 0 100 4m0-4a2 2 0 110 4m-6 8a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4m6 6v10m6-2a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4" />
              </svg>
              <span class="font-medium relative z-10">Settings</span>
              
              <!-- Right arrow indicator for active item -->
              <svg 
                v-if="activeTab === 'settings'" 
                xmlns="http://www.w3.org/2000/svg" 
                class="h-5 w-5 ml-auto relative z-10 text-white" 
                fill="none" 
                viewBox="0 0 24 24" 
                stroke="currentColor"
              >
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
              </svg>
            </a>
          </li>
        </ul>
      </nav>
      
      <!-- Tips Section with Interactive Elements -->
      <div class="p-4 mt-2">
        <div class="bg-gradient-to-br from-gray-100 to-gray-50 dark:from-gray-700 dark:to-gray-800 rounded-lg p-4 shadow-inner relative overflow-hidden">
          <!-- Decorative elements -->
          <div class="absolute top-0 right-0 w-16 h-16 bg-indigo-500 opacity-10 rounded-full -mr-6 -mt-6"></div>
          <div class="absolute bottom-0 left-0 w-12 h-12 bg-purple-500 opacity-10 rounded-full -ml-4 -mb-4"></div>
          
          <h3 class="text-sm font-medium text-gray-800 dark:text-white mb-2 flex items-center">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1 text-indigo-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
            </svg>
            Pro Tip
          </h3>
          
          <div class="space-y-2 text-xs text-gray-600 dark:text-gray-400">
            <p class="flex items-center">
              <span class="inline-flex items-center justify-center w-5 h-5 mr-2 bg-indigo-100 dark:bg-indigo-900 rounded-full text-indigo-800 dark:text-indigo-200">1</span>
              Press <kbd class="px-2 py-1 bg-white dark:bg-gray-600 rounded text-xs mx-1 shadow-sm">Space</kbd> to generate a new palette.
            </p>
            
            <p class="flex items-center">
              <span class="inline-flex items-center justify-center w-5 h-5 mr-2 bg-indigo-100 dark:bg-indigo-900 rounded-full text-indigo-800 dark:text-indigo-200">2</span>
              Click a color to copy its hex code.
            </p>
            
            <p class="flex items-center">
              <span class="inline-flex items-center justify-center w-5 h-5 mr-2 bg-indigo-100 dark:bg-indigo-900 rounded-full text-indigo-800 dark:text-indigo-200">3</span>
              <span class="cursor-pointer hover:text-indigo-600 dark:hover:text-indigo-400 transition-colors" @click="toggleTipsModal">
                View more tips
                <span class="inline-block ml-1 transform transition-transform group-hover:translate-x-1">→</span>
              </span>
            </p>
          </div>
        </div>
      </div>
      
      <!-- Recent Colors Section -->
      <div class="p-4">
        <h3 class="text-sm font-medium text-gray-800 dark:text-white mb-3 flex items-center">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1 text-indigo-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z" />
          </svg>
          Recent Colors
        </h3>
        
        <div class="flex flex-wrap gap-2">
          <div 
            v-for="(color, index) in recentColors" 
            :key="index"
            class="w-8 h-8 rounded-full shadow-sm cursor-pointer transform transition-transform hover:scale-110 hover:shadow-md"
            :style="{ backgroundColor: color }"
            :title="color"
            @click="copyToClipboard(color)"
          ></div>
          
          <div v-if="recentColors.length === 0" class="text-xs text-gray-500 dark:text-gray-400 italic">
            No recent colors yet
          </div>
        </div>
      </div>
      
      <!-- Theme Toggle with Advanced Animation -->
      <div class="absolute bottom-0 left-0 right-0 p-4 border-t border-gray-200 dark:border-gray-700">
        <button 
          @click="toggleTheme" 
          class="w-full flex items-center justify-between p-3 rounded-lg bg-gray-100 dark:bg-gray-700 text-gray-800 dark:text-gray-200 hover:bg-gray-200 dark:hover:bg-gray-600 transition-colors group relative overflow-hidden"
        >
          <div class="absolute inset-0 bg-gradient-to-r from-yellow-100 to-yellow-50 dark:from-indigo-900/30 dark:to-indigo-800/30 opacity-0 group-hover:opacity-100 transition-opacity"></div>
          
          <div class="flex items-center relative z-10">
            <div class="relative">
              <!-- Sun icon with rays animation -->
              <svg v-if="isDarkMode" xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2 text-yellow-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z" />
              </svg>
              
              <!-- Moon icon with stars animation -->
              <svg v-else xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2 text-indigo-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z" />
              </svg>
              
              <!-- Decorative stars for dark mode -->
              <div v-if="!isDarkMode" class="absolute -top-1 -right-1 opacity-0 group-hover:opacity-100 transition-opacity duration-300">
                <svg width="8" height="8" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg" class="animate-pulse">
                  <path d="M12 2L15.09 8.26L22 9.27L17 14.14L18.18 21.02L12 17.77L5.82 21.02L7 14.14L2 9.27L8.91 8.26L12 2Z" fill="#6366F1" />
                </svg>
              </div>
            </div>
            <span class="font-medium">{{ isDarkMode ? 'Light Mode' : 'Dark Mode' }}</span>
          </div>
          
          <!-- Toggle switch -->
          <div class="relative h-6 w-12 rounded-full bg-gray-300 dark:bg-gray-600 transition-colors duration-300 z-10">
            <div 
              class="absolute top-1 left-1 bg-white dark:bg-indigo-500 h-4 w-4 rounded-full transform transition-transform duration-300"
              :class="isDarkMode ? 'translate-x-6' : ''"
            ></div>
          </div>
        </button>
      </div>
    </div>
    
    <!-- Mobile Toggle Button with Animation -->
    <button 
      @click="toggleSidebar" 
      class="md:hidden fixed top-4 right-4 z-30 p-3 rounded-full bg-white dark:bg-gray-800 shadow-lg text-gray-700 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-700 transition-all duration-300 transform hover:scale-110 active:scale-95"
    >
      <svg v-if="isOpen" xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
      </svg>
      <svg v-else xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
      </svg>
    </button>
    
    <!-- Tips Modal -->
    <transition name="modal-fade">
      <div v-if="showTipsModal" class="fixed inset-0 z-50 flex items-center justify-center p-4">
        <div class="absolute inset-0 bg-black bg-opacity-50 backdrop-blur-sm" @click="toggleTipsModal"></div>
        <div class="bg-white dark:bg-gray-800 rounded-xl shadow-2xl max-w-md w-full p-6 relative z-10 transform transition-all">
          <button @click="toggleTipsModal" class="absolute top-4 right-4 text-gray-500 hover:text-gray-700 dark:text-gray-400 dark:hover:text-gray-200">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
            </svg>
          </button>
          
          <h3 class="text-xl font-bold text-gray-900 dark:text-white mb-4">Pro Tips & Shortcuts</h3>
          
          <div class="space-y-4">
            <div class="flex items-start">
              <div class="flex-shrink-0 h-8 w-8 rounded-full bg-indigo-100 dark:bg-indigo-900 flex items-center justify-center text-indigo-600 dark:text-indigo-300 mr-3">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 15l-2 5L9 9l11 4-5 2zm0 0l5 5M7.188 2.239l.777 2.897M5.136 7.965l-2.898-.777M13.95 4.05l-2.122 2.122m-5.657 5.656l-2.12 2.122" />
                </svg>
              </div>
              <div>
                <h4 class="text-sm font-medium text-gray-900 dark:text-white">Navigation</h4>
                <p class="text-sm text-gray-600 dark:text-gray-400">Use the sidebar to switch between different sections of the app.</p>
              </div>
            </div>
            
            <div class="flex items-start">
              <div class="flex-shrink-0 h-8 w-8 rounded-full bg-indigo-100 dark:bg-indigo-900 flex items-center justify-center text-indigo-600 dark:text-indigo-300 mr-3">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6V4m0 2a2 2 0 100 4m0-4a2 2 0 110 4m-6 8a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4m6 6v10m6-2a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4" />
                </svg>
              </div>
              <div>
                <h4 class="text-sm font-medium text-gray-900 dark:text-white">Keyboard Shortcuts</h4>
                <div class="mt-2 space-y-2">
                  <div class="flex items-center">
                    <kbd class="px-2 py-1 bg-gray-100 dark:bg-gray-700 rounded text-xs mr-2">Space</kbd>
                    <span class="text-sm text-gray-600 dark:text-gray-400">Generate new palette</span>
                  </div>
                  <div class="flex items-center">
                    <kbd class="px-2 py-1 bg-gray-100 dark:bg-gray-700 rounded text-xs mr-2">S</kbd>
                    <span class="text-sm text-gray-600 dark:text-gray-400">Save current palette</span>
                  </div>
                  <div class="flex items-center">
                    <kbd class="px-2 py-1 bg-gray-100 dark:bg-gray-700 rounded text-xs mr-2">L</kbd>
                    <span class="text-sm text-gray-600 dark:text-gray-400">Lock/unlock selected color</span>
                  </div>
                </div>
              </div>
            </div>
            
            <div class="flex items-start">
              <div class="flex-shrink-0 h-8 w-8 rounded-full bg-indigo-100 dark:bg-indigo-900 flex items-center justify-center text-indigo-600 dark:text-indigo-300 mr-3">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z" />
                </svg>
              </div>
              <div>
                <h4 class="text-sm font-medium text-gray-900 dark:text-white">Color Theory</h4>
                <p class="text-sm text-gray-600 dark:text-gray-400">Try different harmony modes to create balanced color schemes based on color theory principles.</p>
              </div>
            </div>
          </div>
          
          <div class="mt-6 pt-4 border-t border-gray-200 dark:border-gray-700">
            <button 
              @click="toggleTipsModal" 
              class="w-full py-2 px-4 bg-indigo-600 hover:bg-indigo-700 text-white font-medium rounded-lg transition-colors"
            >
              Got it!
            </button>
          </div>
        </div>
      </div>
    </transition>
    
    <!-- Easter Egg Animation -->
    <transition name="confetti">
      <div v-if="showEasterEgg" class="fixed inset-0 pointer-events-none z-50 flex items-center justify-center">
        <div class="confetti-container">
          <!-- Confetti pieces will be generated here by JS -->
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'Sidebar',
  data() {
    return {
      isOpen: false,
      isDarkMode: false,
      activeTab: 'generator',
      savedPalettesCount: 3,
      recentColors: ['#FF595E', '#FFCA3A', '#8AC926', '#1982C4', '#6A4C93'],
      showTipsModal: false,
      showEasterEgg: false,
      easterEggClickCount: 0
    }
  },
  mounted() {
    // Check for saved theme preference or system preference
    this.isDarkMode = localStorage.getItem('darkMode') === 'true' || 
                      (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches);
    this.applyTheme();
    
    // Check screen size on mount
    this.checkScreenSize();
    
    // Listen for screen size changes
    window.addEventListener('resize', this.checkScreenSize);
    
    // Listen for close sidebar events from App.vue
    this.$root.$on('close-sidebar', () => {
      this.isOpen = false;
    });
    
    // Listen for color updates
    this.$root.$on('color-copied', this.addRecentColor);
  },
  beforeDestroy() {
    // Clean up event listeners
    window.removeEventListener('resize', this.checkScreenSize);
    this.$root.$off('close-sidebar');
    this.$root.$off('color-copied');
  },
  methods: {
    toggleSidebar() {
      this.isOpen = !this.isOpen;
      this.$root.$emit('sidebar-toggled', this.isOpen);
    },
    toggleTheme() {
      this.isDarkMode = !this.isDarkMode;
      localStorage.setItem('darkMode', this.isDarkMode);
      this.applyTheme();
    },
    applyTheme() {
      if (this.isDarkMode) {
        document.documentElement.classList.add('dark');
      } else {
        document.documentElement.classList.remove('dark');
      }
    },
    checkScreenSize() {
      // Open sidebar by default on larger screens
      this.isOpen = window.innerWidth >= 768;
    },
    setActiveTab(tab) {
      this.activeTab = tab;
      // Emit event to parent component to change content
      this.$root.$emit('change-tab', tab);
      
      // On mobile, close sidebar after selection
      if (window.innerWidth < 768) {
        this.isOpen = false;
        this.$root.$emit('sidebar-toggled', false);
      }
    },
    toggleTipsModal() {
      this.showTipsModal = !this.showTipsModal;
      
      // Prevent scrolling on body when modal is open
      if (this.showTipsModal) {
        document.body.style.overflow = 'hidden';
      } else {
        document.body.style.overflow = '';
      }
    },
    copyToClipboard(color) {
      navigator.clipboard.writeText(color).then(() => {
        // Show feedback
        this.$root.$emit('show-notification', `Copied ${color} to clipboard!`);
      });
    },
    addRecentColor(color) {
      // Don't add if already in recent colors
      if (this.recentColors.includes(color)) {
        // Move to front
        this.recentColors = this.recentColors.filter(c => c !== color);
        this.recentColors.unshift(color);
      } else {
        // Add to front, limit to 5 colors
        this.recentColors.unshift(color);
        if (this.recentColors.length > 5) {
          this.recentColors.pop();
        }
      }
    },
    triggerEasterEgg() {
      this.easterEggClickCount++;
      
      if (this.easterEggClickCount >= 5) {
        this.showEasterEgg = true;
        this.createConfetti();
        
        // Reset after animation
        setTimeout(() => {
          this.showEasterEgg = false;
          this.easterEggClickCount = 0;
        }, 3000);
      }
    },
    createConfetti() {
      const confettiContainer = document.querySelector('.confetti-container');
      if (!confettiContainer) return;
      
      // Clear previous confetti
      confettiContainer.innerHTML = '';
      
      // Create confetti pieces
      const colors = ['#FF595E', '#FFCA3A', '#8AC926', '#1982C4', '#6A4C93'];
      
      for (let i = 0; i < 100; i++) {
        const confetti = document.createElement('div');
        confetti.className = 'confetti-piece';
        
        // Random styling
        const color = colors[Math.floor(Math.random() * colors.length)];
        const size = Math.random() * 10 + 5;
        const left = Math.random() * 100;
        const animationDuration = Math.random() * 3 + 2;
        const animationDelay = Math.random() * 0.5;
        
        confetti.style.backgroundColor = color;
        confetti.style.width = `${size}px`;
        confetti.style.height = `${size}px`;
        confetti.style.left = `${left}vw`;
        confetti.style.animationDuration = `${animationDuration}s`;
        confetti.style.animationDelay = `${animationDelay}s`;
        
        confettiContainer.appendChild(confetti);
      }
    }
  }
}
</script>

<style scoped>
/* Base sidebar styles */
.sidebar {
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
}

/* Logo animations */
.color-swatches .swatch {
  transition: transform 0.5s ease;
  transform-origin: center;
}

.logo-svg:hover .swatch-1 {
  transform: translate(-3px, -3px);
}

.logo-svg:hover .swatch-2 {
  transform: translate(3px, -3px);
}

.logo-svg:hover .swatch-3 {
  transform: translate(-3px, 3px);
}

.logo-svg:hover .swatch-4 {
  transform: translate(3px, 3px);
}

.center-circle {
  transition: transform 0.5s ease;
}

.logo-svg:hover .center-circle {
  transform: scale(1.2);
}

/* Modal transitions */
.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity 0.3s, transform 0.3s;
}

.modal-fade-enter-from,
.modal-fade-leave-to {
  opacity: 0;
  transform: scale(0.95);
}

/* Confetti animation */
.confetti-enter-active {
  transition: opacity 0.5s;
}

.confetti-leave-active {
  transition: opacity 1s;
}

.confetti-enter-from,
.confetti-leave-to {
  opacity: 0;
}

.confetti-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  pointer-events: none;
}

.confetti-piece {
  position: absolute;
  top: -20px;
  width: 10px;
  height: 10px;
  background-color: #f00;
  border-radius: 2px;
  animation: confetti-fall linear forwards;
}

@keyframes confetti-fall {
  0% {
    transform: translateY(0) rotate(0deg);
    opacity: 1;
  }
  100% {
    transform: translateY(100vh) rotate(720deg);
    opacity: 0;
  }
}

/* Ensure the sidebar has proper z-index and transitions */
.sidebar {
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

/* Responsive adjustments */
@media (max-width: 768px) {
  .sidebar {
    width: 85%;
    max-width: 320px;
  }
}
</style>
