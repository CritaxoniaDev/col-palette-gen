<template>
  <div class="color-generator max-w-6xl mx-auto" tabindex="0" @keydown.space.prevent="generatePalette" ref="generator">
    <!-- Logo -->
    <div class="flex justify-center mb-8">
      <div class="logo-container">
        <svg width="120" height="120" viewBox="0 0 120 120" xmlns="http://www.w3.org/2000/svg" class="logo-svg">
          <!-- Color wheel background -->
          <circle cx="60" cy="60" r="50" fill="#f8f9fa" class="dark:fill-gray-800" />

          <!-- Color palette elements -->
          <g class="palette-elements">
            <!-- Color swatches that rotate on hover -->
            <rect x="30" y="35" width="25" height="25" rx="4" fill="#FF595E" transform-origin="60 60"
              class="swatch swatch-1" />
            <rect x="65" y="35" width="25" height="25" rx="4" fill="#FFCA3A" transform-origin="60 60"
              class="swatch swatch-2" />
            <rect x="30" y="65" width="25" height="25" rx="4" fill="#8AC926" transform-origin="60 60"
              class="swatch swatch-3" />
            <rect x="65" y="65" width="25" height="25" rx="4" fill="#1982C4" transform-origin="60 60"
              class="swatch swatch-4" />

            <!-- Center circle -->
            <circle cx="60" cy="60" r="12" fill="#6A4C93" class="center-circle" />

            <!-- Paintbrush handle -->
            <rect x="57" y="75" width="6" height="25" rx="2" fill="#4A4A4A" class="dark:fill-gray-300" />
          </g>
        </svg>
      </div>
    </div>
    <!-- Color Palette Display -->
    <div class="mb-12">
      <!-- Main Palette - Side by side with no gaps -->
      <div class="mb-6">
        <div class="flex flex-col md:flex-row overflow-hidden shadow-2xl">
          <div v-for="(color, index) in palette" :key="index"
            class="color-card relative flex-1 h-24 md:h-80 transition-all duration-300 cursor-pointer group"
            :style="{ backgroundColor: color.hex }" @click="copyToClipboard(color.hex)">
            <div class="absolute inset-0 bg-gradient-to-b from-transparent via-transparent to-black opacity-10"></div>
            <div
              class="absolute bottom-0 left-0 right-0 bg-white/80 dark:bg-gray-900/80 p-3 transform translate-y-full group-hover:translate-y-0 transition-transform duration-200">
              <p class="font-mono text-sm font-medium text-gray-800 dark:text-gray-200">{{ color.hex }}</p>
              <p class="text-xs text-gray-600 dark:text-gray-400 mt-1">Click to copy</p>
            </div>
            <div v-if="copiedIndex === index"
              class="absolute inset-0 flex items-center justify-center bg-black/60 backdrop-blur-sm transition-opacity duration-300">
              <span class="text-white font-medium px-4 py-2 rounded-full bg-green-500/90 shadow-lg flex items-center">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" fill="none" viewBox="0 0 24 24"
                  stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
                </svg>
                Copied!
              </span>
            </div>

            <!-- Remove color button -->
            <button v-if="palette.length > 2" @click.stop="removeColor(index)"
              class="absolute top-2 right-2 h-8 w-8 rounded-full bg-white/80 dark:bg-gray-900/80 shadow-md flex items-center justify-center opacity-0 group-hover:opacity-100 transition-opacity duration-200 hover:bg-white dark:hover:bg-gray-800"
              title="Remove color">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-gray-700 dark:text-gray-300" fill="none"
                viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
              </svg>
            </button>
          </div>
        </div>

        <!-- Color count controls -->
        <div class="flex justify-center mt-4 space-x-4">
          <button @click="removeColor()"
            class="p-2 rounded-lg bg-white dark:bg-gray-800 shadow-md text-gray-700 dark:text-gray-300 disabled:opacity-50 disabled:cursor-not-allowed flex items-center"
            :disabled="palette.length <= 2" title="Remove color">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24"
              stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20 12H4" />
            </svg>
          </button>

          <div
            class="px-4 py-2 bg-white dark:bg-gray-800 rounded-lg shadow-md text-gray-800 dark:text-gray-200 flex items-center">
            <span class="font-medium">{{ palette.length }}</span>
            <span class="ml-1 text-sm text-gray-500 dark:text-gray-400">colors</span>
          </div>

          <button @click="addColor()"
            class="p-2 rounded-lg bg-white dark:bg-gray-800 shadow-md text-gray-700 dark:text-gray-300 disabled:opacity-50 disabled:cursor-not-allowed flex items-center"
            :disabled="palette.length >= 5" title="Add color">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24"
              stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6v6m0 0v6m0-6h6m-6 0H6" />
            </svg>
          </button>
        </div>
      </div>

      <!-- Spacebar Instruction -->
      <div class="text-center mb-8">
        <div
          class="inline-flex items-center px-4 py-2 bg-gray-100 dark:bg-gray-800 rounded-lg text-gray-700 dark:text-gray-300 text-sm">
          <span class="mr-2">Press</span>
          <kbd
            class="px-4 py-1 bg-white dark:bg-gray-700 border border-gray-300 dark:border-gray-600 rounded shadow-sm text-gray-800 dark:text-gray-200 font-mono">Space</kbd>
          <span class="ml-2">to generate a new palette</span>
        </div>
      </div>

      <!-- Recommended Palettes -->
      <h3 class="text-lg font-semibold text-gray-900 dark:text-white mb-4 flex items-center">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2 text-indigo-500" fill="none" viewBox="0 0 24 24"
          stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z" />
        </svg>
        Recommended Combinations
      </h3>

      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <!-- Recommendation 1 -->
        <div class="bg-white dark:bg-gray-800 rounded-xl shadow-lg overflow-hidden">
          <div class="flex h-16">
            <div v-for="(color, index) in recommendedPalettes[0]" :key="'rec1-' + index" class="flex-1 h-full"
              :style="{ backgroundColor: color }" @click="copyToClipboard(color)" title="Click to copy"></div>
          </div>
          <div class="p-4">
            <h4 class="font-medium text-gray-900 dark:text-white">Elegant Harmony</h4>
            <p class="text-sm text-gray-600 dark:text-gray-400">Perfect for sophisticated interfaces</p>
          </div>
        </div>

        <!-- Recommendation 2 -->
        <div class="bg-white dark:bg-gray-800 rounded-xl shadow-lg overflow-hidden">
          <div class="flex h-16">
            <div v-for="(color, index) in recommendedPalettes[1]" :key="'rec2-' + index" class="flex-1 h-full"
              :style="{ backgroundColor: color }" @click="copyToClipboard(color)" title="Click to copy"></div>
          </div>
          <div class="p-4">
            <h4 class="font-medium text-gray-900 dark:text-white">Vibrant Energy</h4>
            <p class="text-sm text-gray-600 dark:text-gray-400">Bold colors for creative projects</p>
          </div>
        </div>

        <!-- Recommendation 3 -->
        <div class="bg-white dark:bg-gray-800 rounded-xl shadow-lg overflow-hidden">
          <div class="flex h-16">
            <div v-for="(color, index) in recommendedPalettes[2]" :key="'rec3-' + index" class="flex-1 h-full"
              :style="{ backgroundColor: color }" @click="copyToClipboard(color)" title="Click to copy"></div>
          </div>
          <div class="p-4">
            <h4 class="font-medium text-gray-900 dark:text-white">Calm Neutrals</h4>
            <p class="text-sm text-gray-600 dark:text-gray-400">Subtle tones for balanced designs</p>
          </div>
        </div>

        <!-- Recommendation 4 -->
        <div class="bg-white dark:bg-gray-800 rounded-xl shadow-lg overflow-hidden">
          <div class="flex h-16">
            <div v-for="(color, index) in recommendedPalettes[3]" :key="'rec4-' + index" class="flex-1 h-full"
              :style="{ backgroundColor: color }" @click="copyToClipboard(color)" title="Click to copy"></div>
          </div>
          <div class="p-4">
            <h4 class="font-medium text-gray-900 dark:text-white">Natural Essence</h4>
            <p class="text-sm text-gray-600 dark:text-gray-400">Earth-inspired color harmony</p>
          </div>
        </div>
      </div>
    </div>

    <!-- Controls -->
    <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-lg p-6 mb-10">
      <div class="flex flex-col md:flex-row gap-8">
        <div class="flex-1">
          <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-3">Color Mode</label>
          <div class="flex flex-wrap gap-3">
            <button @click="mode = 'random'"
              class="px-5 py-2.5 rounded-lg text-sm font-medium transition-all duration-200 border"
              :class="mode === 'random'
                ? 'bg-indigo-600 text-white border-indigo-700 shadow-md shadow-indigo-200 dark:shadow-none'
                : 'bg-gray-100 dark:bg-gray-700 text-gray-800 dark:text-gray-200 hover:bg-gray-200 dark:hover:bg-gray-600 border-gray-200 dark:border-gray-600'">
              <div class="flex items-center">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1.5" fill="none" viewBox="0 0 24 24"
                  stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="M19.428 15.428a2 2 0 00-1.022-.547l-2.387-.477a6 6 0 00-3.86.517l-.318.158a6 6 0 01-3.86.517L6.05 15.21a2 2 0 00-1.806.547M8 4h8l-1 1v5.172a2 2 0 00.586 1.414l5 5c1.26 1.26.367 3.414-1.415 3.414H4.828c-1.782 0-2.674-2.154-1.414-3.414l5-5A2 2 0 009 10.172V5L8 4z" />
                </svg>
                Random
              </div>
            </button>
            <button @click="mode = 'monochromatic'"
              class="px-5 py-2.5 rounded-lg text-sm font-medium transition-all duration-200 border"
              :class="mode === 'monochromatic'
                ? 'bg-indigo-600 text-white border-indigo-700 shadow-md shadow-indigo-200 dark:shadow-none'
                : 'bg-gray-100 dark:bg-gray-700 text-gray-800 dark:text-gray-200 hover:bg-gray-200 dark:hover:bg-gray-600 border-gray-200 dark:border-gray-600'">
              <div class="flex items-center">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1.5" fill="none" viewBox="0 0 24 24"
                  stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="M7 21a4 4 0 01-4-4V5a2 2 0 012-2h4a2 2 0 012 2v12a4 4 0 01-4 4zm0 0h12a2 2 0 002-2v-4a2 2 0 00-2-2h-2.343M11 7.343l1.657-1.657a2 2 0 012.828 0l2.829 2.829a2 2 0 010 2.828l-8.486 8.485M7 17h.01" />
                </svg>
                Monochromatic
              </div>
            </button>
            <button @click="mode = 'analogous'"
              class="px-5 py-2.5 rounded-lg text-sm font-medium transition-all duration-200 border"
              :class="mode === 'analogous'
                ? 'bg-indigo-600 text-white border-indigo-700 shadow-md shadow-indigo-200 dark:shadow-none'
                : 'bg-gray-100 dark:bg-gray-700 text-gray-800 dark:text-gray-200 hover:bg-gray-200 dark:hover:bg-gray-600 border-gray-200 dark:border-gray-600'">
              <div class="flex items-center">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1.5" fill="none" viewBox="0 0 24 24"
                  stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="M14 10l-2 1m0 0l-2-1m2 1v2.5M20 7l-2 1m2-1l-2-1m2 1v2.5M14 4l-2-1-2 1M4 7l2-1M4 7l2 1M4 7v2.5M12 21l-2-1m2 1l2-1m-2 1v-2.5M6 18l-2-1v-2.5M18 18l2-1v-2.5" />
                </svg>
                Analogous
              </div>
            </button>
          </div>
        </div>
        <div class="flex-1 mt-6 md:mt-0">
          <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-3">Base Color</label>
          <div class="flex items-center space-x-4">
            <div class="relative">
              <input type="color" v-model="baseColor"
                class="h-12 w-12 rounded-lg border-0 cursor-pointer appearance-none overflow-hidden" />
              <div class="absolute inset-0 rounded-lg pointer-events-none shadow-md"></div>
            </div>
            <input type="text" v-model="baseColor"
              class="flex-1 px-4 py-3 border border-gray-300 dark:border-gray-600 rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 dark:bg-gray-800 dark:text-white text-sm font-mono"
              placeholder="#FFFFFF" />
          </div>
        </div>
      </div>
    </div>

    <!-- Tips Section -->
    <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-lg p-8 mb-8">
      <h3 class="text-xl font-semibold text-gray-900 dark:text-white mb-5 flex items-center">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2 text-indigo-500" fill="none" viewBox="0 0 24 24"
          stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
            d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
        </svg>
        Tips for Using Color Palettes
      </h3>
      <ul class="space-y-3 text-gray-600 dark:text-gray-300">
        <li class="flex items-start p-2 rounded-lg hover:bg-gray-50 dark:hover:bg-gray-700/50 transition-colors">
          <span
            class="flex-shrink-0 h-6 w-6 flex items-center justify-center rounded-full bg-indigo-100 dark:bg-indigo-900/30 text-indigo-600 dark:text-indigo-400 mr-3 mt-0.5">1</span>
          <span>Use your dominant color for primary elements, and accent colors sparingly</span>
        </li>
        <li class="flex items-start p-2 rounded-lg hover:bg-gray-50 dark:hover:bg-gray-700/50 transition-colors">
          <span
            class="flex-shrink-0 h-6 w-6 flex items-center justify-center rounded-full bg-indigo-100 dark:bg-indigo-900/30 text-indigo-600 dark:text-indigo-400 mr-3 mt-0.5">2</span>
          <span>Consider accessibility and ensure sufficient contrast for text</span>
        </li>
        <li class="flex items-start p-2 rounded-lg hover:bg-gray-50 dark:hover:bg-gray-700/50 transition-colors">
          <span
            class="flex-shrink-0 h-6 w-6 flex items-center justify-center rounded-full bg-indigo-100 dark:bg-indigo-900/30 text-indigo-600 dark:text-indigo-400 mr-3 mt-0.5">3</span>
          <span>Test your palette in both light and dark environments</span>
        </li>
        <li class="flex items-start p-2 rounded-lg hover:bg-gray-50 dark:hover:bg-gray-700/50 transition-colors">
          <span
            class="flex-shrink-0 h-6 w-6 flex items-center justify-center rounded-full bg-indigo-100 dark:bg-indigo-900/30 text-indigo-600 dark:text-indigo-400 mr-3 mt-0.5">4</span>
          <span>Save palettes you like for future reference</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ColorGenerator',
  data() {
    return {
      palette: [],
      mode: 'random',
      baseColor: '#4F46E5',
      copiedIndex: null,
      recommendedPalettes: [
        ['#264653', '#2a9d8f', '#e9c46a', '#f4a261', '#e76f51'],
        ['#ff595e', '#ffca3a', '#8ac926', '#1982c4', '#6a4c93'],
        ['#f8f9fa', '#e9ecef', '#dee2e6', '#ced4da', '#adb5bd'],
        ['#606c38', '#283618', '#fefae0', '#dda15e', '#bc6c25']
      ]
    }
  },
  mounted() {
    this.generatePalette()
    // Set focus to the component to enable spacebar detection
    this.$refs.generator.focus()

    // Add event listener for when the window regains focus
    window.addEventListener('focus', this.setFocus)
  },
  beforeDestroy() {
    // Clean up event listener
    window.removeEventListener('focus', this.setFocus)
  },
  // Modify the existing generatePalette method to respect the current count
  generatePalette() {
    // Reset copied state
    this.copiedIndex = null;

    const colorCount = this.palette.length;

    if (this.mode === 'random') {
      this.generateRandomPalette(colorCount);
    } else if (this.mode === 'monochromatic') {
      this.generateMonochromaticPalette(colorCount);
    } else if (this.mode === 'analogous') {
      this.generateAnalogousPalette(colorCount);
    }
  },

  // Update these methods to accept a count parameter
  generateRandomPalette(count = 5) {
    const palette = [];
    for (let i = 0; i < count; i++) {
      const hex = this.getRandomHexColor();
      palette.push({ hex });
    }
    this.palette = palette;
  },

  generateMonochromaticPalette(count = 5) {
    const palette = [];
    const hsl = this.hexToHSL(this.baseColor);

    for (let i = 0; i < count; i++) {
      // Distribute lightness evenly based on count
      const lightness = 20 + (i * (60 / (count - 1)));
      const hex = this.HSLToHex(hsl.h, hsl.s, lightness);
      palette.push({ hex });
    }

    this.palette = palette;
  },

  generateAnalogousPalette(count = 5) {
    const palette = [];
    const hsl = this.hexToHSL(this.baseColor);

    // Create colors with hues spaced around the color wheel
    const spread = Math.min(30, 120 / (count - 1));
    for (let i = 0; i < count; i++) {
      const hue = (hsl.h + (i - Math.floor(count / 2)) * spread + 360) % 360;
      const hex = this.HSLToHex(hue, hsl.s, hsl.l);
      palette.push({ hex });
    }

    this.palette = palette;
  },
  methods: {
    setFocus() {
      // Refocus the component when window regains focus
      this.$refs.generator.focus()
    },
    generatePalette() {
      // Reset copied state
      this.copiedIndex = null

      if (this.mode === 'random') {
        this.generateRandomPalette()
      } else if (this.mode === 'monochromatic') {
        this.generateMonochromaticPalette()
      } else if (this.mode === 'analogous') {
        this.generateAnalogousPalette()
      }
    },

    addColor() {
      if (this.palette.length < 5) {
        let newColor;

        if (this.mode === 'random') {
          newColor = { hex: this.getRandomHexColor() };
        } else if (this.mode === 'monochromatic') {
          const hsl = this.hexToHSL(this.baseColor);
          const lightness = 20 + (this.palette.length * 15);
          newColor = { hex: this.HSLToHex(hsl.h, hsl.s, lightness) };
        } else if (this.mode === 'analogous') {
          const hsl = this.hexToHSL(this.baseColor);
          const hue = (hsl.h + (this.palette.length - 2) * 30 + 360) % 360;
          newColor = { hex: this.HSLToHex(hue, hsl.s, hsl.l) };
        }

        this.palette.push(newColor);
      }
    },

    removeColor(index) {
      if (this.palette.length > 2) {
        if (index !== undefined) {
          // Remove specific color
          this.palette.splice(index, 1);
        } else {
          // Remove last color
          this.palette.pop();
        }

        // Reset copied index if needed
        if (this.copiedIndex >= this.palette.length) {
          this.copiedIndex = null;
        }
      }
    },

    generateRandomPalette() {
      const palette = []
      for (let i = 0; i < 5; i++) {
        const hex = this.getRandomHexColor()
        palette.push({ hex })
      }
      this.palette = palette
    },

    generateMonochromaticPalette() {
      const palette = []
      const hsl = this.hexToHSL(this.baseColor)

      for (let i = 0; i < 5; i++) {
        // Vary lightness for monochromatic palette
        const lightness = 20 + (i * 15)
        const hex = this.HSLToHex(hsl.h, hsl.s, lightness)
        palette.push({ hex })
      }

      this.palette = palette
    },

    generateAnalogousPalette() {
      const palette = []
      const hsl = this.hexToHSL(this.baseColor)

      // Create colors with hues spaced around the color wheel
      for (let i = 0; i < 5; i++) {
        const hue = (hsl.h + (i - 2) * 30 + 360) % 360
        const hex = this.HSLToHex(hue, hsl.s, hsl.l)
        palette.push({ hex })
      }

      this.palette = palette
    },

    getRandomHexColor() {
      return '#' + Math.floor(Math.random() * 16777215).toString(16).padStart(6, '0')
    },

    hexToHSL(hex) {
      // Remove # if present
      hex = hex.replace(/^#/, '')

      // Parse the hex values
      const r = parseInt(hex.substring(0, 2), 16) / 255
      const g = parseInt(hex.substring(2, 4), 16) / 255
      const b = parseInt(hex.substring(4, 6), 16) / 255

      const max = Math.max(r, g, b)
      const min = Math.min(r, g, b)
      let h, s, l = (max + min) / 2

      if (max === min) {
        h = s = 0 // achromatic
      } else {
        const d = max - min
        s = l > 0.5 ? d / (2 - max - min) : d / (max + min)

        switch (max) {
          case r: h = (g - b) / d + (g < b ? 6 : 0); break
          case g: h = (b - r) / d + 2; break
          case b: h = (r - g) / d + 4; break
        }

        h = h * 60
      }

      return { h, s: s * 100, l: l * 100 }
    },

    HSLToHex(h, s, l) {
      s /= 100
      l /= 100

      const c = (1 - Math.abs(2 * l - 1)) * s
      const x = c * (1 - Math.abs((h / 60) % 2 - 1))
      const m = l - c / 2
      let r, g, b

      if (0 <= h && h < 60) {
        r = c; g = x; b = 0
      } else if (60 <= h && h < 120) {
        r = x; g = c; b = 0
      } else if (120 <= h && h < 180) {
        r = 0; g = c; b = x
      } else if (180 <= h && h < 240) {
        r = 0; g = x; b = c
      } else if (240 <= h && h < 300) {
        r = x; g = 0; b = c
      } else {
        r = c; g = 0; b = x
      }

      r = Math.round((r + m) * 255).toString(16).padStart(2, '0')
      g = Math.round((g + m) * 255).toString(16).padStart(2, '0')
      b = Math.round((b + m) * 255).toString(16).padStart(2, '0')

      return `#${r}${g}${b}`
    },

    copyToClipboard(text) {
      navigator.clipboard.writeText(text).then(() => {
        // Find the index of the copied color
        const index = this.palette.findIndex(color => color.hex === text)
        if (index !== -1) {
          this.copiedIndex = index

          // Reset the copied state after 1.5 seconds
          setTimeout(() => {
            this.copiedIndex = null
          }, 1500)
        }
      })
    }
  }
}
</script>

<style scoped>
/* Add some styling to make the component focusable without visible outline */
.color-generator {
  outline: none;
}

/* Custom styling for color picker */
input[type="color"] {
  -webkit-appearance: none;
  border: none;
}

input[type="color"]::-webkit-color-swatch-wrapper {
  padding: 0;
}

input[type="color"]::-webkit-color-swatch {
  border: none;
  border-radius: 0.5rem;
}
</style>
