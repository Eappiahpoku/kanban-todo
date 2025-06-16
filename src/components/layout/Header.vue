<!--
  Header.vue
  Stratonea-standard app header for Kanban To-Do List App.
  - Mobile-first, accessible, and touch-optimized.
  - Logo left, app name right.
  - Uses Tailwind utility classes for styling.
  - Follows Stratonea guidelines for Ghanaian context.
-->

<template>
  <!-- ===== [New Feature] START ===== -->
  <header
    class="flex items-center justify-between px-4 py-3 bg-primary text-white shadow-md"
    role="banner"
    aria-label="App Header"
  >
    <!-- Logo: always on the left, sized for mobile -->
    <StratoneaLogo
      class="h-10 w-auto"
      alt="Stratonea Logo"
      aria-label="Stratonea Home"
      white
    />
    <!-- App Name: right, bold, accessible -->
    <span
      class="text-lg font-bold tracking-wide"
      aria-label="App Name"
    >
      Kanban To-Do List
    </span>
  </header>
  <!-- ===== [New Feature] END ===== -->
</template>

<script setup lang="ts">
// ===== Imports & Config =====
/**
 * Imports the Stratonea logo component.
 * - Must exist at @/components/base/StratoneaLogo.vue
 * - Passes 'white' prop for white logo variant (per branding)
 */
import StratoneaLogo from '@/components/base/StratoneaLogo.vue'
</script>

<!--
  All styling is via Tailwind utility classes for maintainability and mobile-first workflow.
  - bg-primary: uses Stratonea brand color (see tailwind.config.js)
  - h-10: 40px height for mobile touch target (min 32px, max 48px)
  - px-4 py-3: mobile-friendly padding
  - shadow-md: subtle elevation for header
  - text-lg font-bold: clear, accessible app name
-->