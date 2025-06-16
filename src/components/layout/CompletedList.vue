<!--
  CompletedList.vue
  Displays a list of completed tasks.
  - Receives an array of completed tasks as a prop.
  - Simple list format with completed indicator.
  - Follows Stratonea guidelines (mobile-first, Tailwind, TypeScript, accessibility).
-->

<template>
  <!-- ===== [New Feature] START ===== -->
  <!-- Container for the completed tasks list -->
  <div
    class="flex flex-col p-4 bg-gray-100 rounded-lg shadow-inner mt-4"
    aria-label="Completed Tasks List"
    role="region"
  >
    <!-- Title for the list -->
    <h3 class="text-lg font-semibold mb-3 text-gray-800" aria-label="Completed tasks title">
      Completed Tasks ({{ completedTasks.length }})
    </h3>

    <!-- List of completed tasks -->
    <div v-if="completedTasks.length > 0" class="flex flex-col gap-2" role="list">
      <!-- Iterate over each completed task -->
      <div
        v-for="task in completedTasks"
        :key="task.id"
        class="flex items-center p-3 bg-white rounded shadow-sm text-gray-600 line-through"
        aria-label="Completed task"
        role="listitem"
      >
        <!-- Checkmark or completed indicator -->
        <span class="mr-2 text-green-600" aria-hidden="true">✅</span>
        <!-- Task Title -->
        <span class="flex-1 truncate">{{ task.title }}</span>
        <!-- Could add more details or actions here later -->
      </div>
    </div>

    <!-- Message when no tasks are completed -->
    <div v-else class="text-gray-500 italic text-center py-4">
      No tasks completed yet.
    </div>
  </div>
  <!-- ===== [New Feature] END ===== -->
</template>

<script setup lang="ts">
// ===== Imports & Config =====
// No external imports needed for this simple component.

// ===== Types & Interfaces =====
/**
 * Defines the structure for a Task object, including completed status.
 * Note: This interface should ideally be shared globally or via a common file
 * when the project grows. Redefined here for clarity in this component for now.
 */
interface Task {
  id: string; // Unique identifier
  title: string;
  column: string; // 'my-day', 'my-week', 'masterlist'
  completed: boolean; // Indicates if the task is completed
  // Add other properties as needed (description, dueDate, priority, etc.)
}

/**
 * Defines the props accepted by the CompletedList component.
 * - completedTasks: An array of Task objects that are completed.
 */
interface Props {
  completedTasks: Task[]; // Array of completed Task objects
}

// ===== Props & Emits =====
// Define props with TypeScript types and default values
withDefaults(defineProps<Props>(), {
  completedTasks: () => [], // Default to an empty array
});

// No events are emitted by this component as it's purely for display.

// ===== State & Computed Properties =====
// No internal state or complex computed properties needed for simple display.

// ===== Watchers =====
// No watchers needed.

// ===== Methods =====
// No methods needed.

</script>

<!--
  Tailwind CSS Styling Notes:
  - flex flex-col: Stacks title and list vertically.
  - p-4 bg-gray-100 rounded-lg shadow-inner mt-4: Styling for the container, similar to TaskBoard for consistency.
  - text-lg font-semibold mb-3 text-gray-800: Styling for the list title.
  - flex flex-col gap-2: Stacks task items vertically with spacing.
  - p-3 bg-white rounded shadow-sm: Styling for each task item container.
  - text-gray-600 line-through: Styles for completed task text (grayed out and struck through).
  - flex items-center: Aligns checkmark and text horizontally.
  - mr-2 text-green-600: Styling for the checkmark icon.
  - flex-1 truncate: Allows the title to take available space and truncate if too long.
  - text-gray-500 italic text-center py-4: Styling for the "No tasks completed" message.
  - aria-* attributes: Added for accessibility.
-->