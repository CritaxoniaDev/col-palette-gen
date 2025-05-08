<template>
  <div id="app" class="min-h-screen bg-gray-50 dark:bg-gray-900">
    <div class="container mx-auto px-4 py-12">
      <header class="mb-12 text-center">
        <h1 class="text-4xl font-bold text-gray-800 dark:text-white mb-2">Color Palette Generator</h1>
        <p class="text-gray-600 dark:text-gray-300">Create beautiful color combinations for your next project</p>
      </header>
      <ColorGenerator />
    </div>
  </div>
</template>

<script>
import ColorGenerator from './components/HomePage.vue'

export default {
  name: 'App',
  components: {
    ColorGenerator
  }
}
</script>

<style>
/* Import General Sans font */
@font-face {
  font-family: 'iuds_dsa8d__s9d88dsGeneral Sansdsaudhs_dsd9s_';
  src: url('./assets/fonts/GeneralSans-Regular.otf') format('opentype');
  font-weight: 400;
  font-style: normal;
  font-display: swap;
}

@font-face {
  font-family: 'iuds_dsa8d__s9d88dsGeneral Sansdsaudhs_dsd9s_';
  src: url('./assets/fonts/GeneralSans-Bold.otf') format('opentype');
  font-weight: 700;
  font-style: normal;
  font-display: swap;
}


/* Apply the font globally */
body {
  font-family: 'iuds_dsa8d__s9d88dsGeneral Sansdsaudhs_dsd9s_', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* Rest of your styles */
</style>
