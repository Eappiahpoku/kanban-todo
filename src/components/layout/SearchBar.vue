<!--
  SearchBar.vue
  Search input component for filtering tasks.
  - Designed for integration into the header area.
  - Mobile-first, accessible, and uses Tailwind utility classes.
  - Follows Stratonea guidelines for simplicity and touch targets.
-->

<template>
  <!-- ===== [New Feature] START ===== -->
  <!-- Search Container: Flex layout for icon and input, responsive padding -->
  <div
    class="flex items-center bg-white rounded-lg shadow-sm px-3 py-2 w-full max-w-xs md:max-w-sm"
    role="search"
    aria-label="Task Search"
  >
    <!-- Search Icon (Placeholder) -->
    <!-- In a real app, you'd use an SVG icon component here -->
    <span class="text-gray-400 mr-2" aria-hidden="true">🔍</span>

    <!-- Search Input -->
    <!-- Using v-model for easy two-way binding in parent -->
    <!-- touch-target-min is applied via padding/height of the container -->
    <input
      type="text"
      :value="modelValue"
      @input="onInput"
      
      :placeholder="placeholder || ''"
      
      class="flex-1 outline-none border-none focus:ring-0 text-gray-700 text-base bg-transparent"
      aria-label="Search tasks by title or description"
      role="searchbox"
    />
  </div>
  <!-- ===== [New Feature] END ===== -->
</template>

<script setup lang="ts">
// ===== Imports & Config =====
// No external imports needed for this simple component.

// ===== Types & Interfaces =====
/**
 * Defines the props accepted by the SearchBar component.
 * - modelValue: The current value of the search input (for v-model).
 * - placeholder: The placeholder text for the input.
 */
interface Props {
  modelValue: string; // The current search query string
  placeholder?: string; // Placeholder text for the input field
}

// Define props with TypeScript types and default values
withDefaults(defineProps<Props>(), {
  modelValue: '', // Default to an empty string
  placeholder: 'Search tasks...', // Default placeholder text
});

// Define the events emitted by the SearchBar component.
// - update:modelValue: Emitted when the input value changes (for v-model).
// - search: Emitted when the user might want to trigger a search (e.g., on Enter key, though not implemented here yet).
const emit = defineEmits<{
  (e: 'update:modelValue', value: string): void;
  (e: 'search', value: string): void; // Optional: for explicit search trigger
}>();

// ===== Main Logic =====
/**
 * Handles the input event from the text field.
 * Emits the 'update:modelValue' event to support v-model binding.
 * Also emits a 'search' event (optional, could be used for debouncing or explicit search).
 */
function onInput(event: Event) {
  const target = event.target as HTMLInputElement;
  const value = target.value;
  emit('update:modelValue', value);
  // Optionally emit a 'search' event, e.g., after a delay or on Enter key
  // emit('search', value);
}

</script>

<!--
  Tailwind CSS Styling Notes:
  - flex items-center: Aligns the icon and input horizontally.
  - bg-white rounded-lg shadow-sm: Background, rounded corners, and subtle shadow for the container.
  - px-3 py-2: Padding around the content. This contributes to the touch target size.
  - w-full max-w-xs md:max-w-sm: Full width on mobile, limited max width on larger screens.
  - text-gray-400 mr-2: Styling for the icon placeholder.
  - flex-1: Allows the input to grow and take available space.
  - outline-none border-none focus:ring-0: Removes default input styling for a cleaner look.
  - text-gray-700 text-base bg-transparent: Text color, size, and transparent background for the input.
  - The container's padding (py-2) helps ensure the overall interactive area meets the touch target guidelines.
-->