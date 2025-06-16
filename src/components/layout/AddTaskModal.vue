<!--
  AddTaskModal.vue
  Modal component for adding a new task.
  - Collects task details: Title, Description, Due Date, Priority.
  - Mobile-first design with large touch targets.
  - Uses native HTML inputs where appropriate.
  - Follows Stratonea guidelines (Tailwind, TypeScript, simple UI, accessibility).
-->

<template>
  <!-- ===== [New Feature] START ===== -->
  <!-- Modal Overlay: Fixed position, covers the screen, semi-transparent background -->
  <!-- Use v-if to mount/unmount the modal for performance -->
  <div
    v-if="modelValue"
    class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4 z-50"
    aria-modal="true"
    role="dialog"
    aria-labelledby="modal-title"
  >
    <!-- Modal Content: White background, rounded corners, padding, max width -->
    <div
      class="bg-white rounded-lg p-6 w-full max-w-sm md:max-w-md shadow-xl flex flex-col max-h-[90vh] overflow-y-auto"
      role="document"
    >
      <!-- Modal Title -->
      <h3 id="modal-title" class="text-xl font-semibold mb-4 text-gray-800">Add New Task</h3>

      <!-- Task Form -->
      <form @submit.prevent="saveTask" class="flex flex-col gap-4">
        <!-- Title Input -->
        <div>
          <label for="task-title" class="block text-sm font-medium text-gray-700 mb-1">Title</label>
          <input
            type="text"
            id="task-title"
            v-model="newTask.title"
            required
            class="block w-full px-3 py-3 rounded-lg border border-gray-300 focus:ring-primary-500 focus:border-primary-500 text-base bg-white shadow-sm touch-target-min"
            aria-required="true"
            aria-label="Task Title"
          />
        </div>

        <!-- Description Input -->
        <div>
          <label for="task-description" class="block text-sm font-medium text-gray-700 mb-1">Description (Optional)</label>
          <textarea
            id="task-description"
            v-model="newTask.description"
            rows="3"
            class="block w-full px-3 py-3 rounded-lg border border-gray-300 focus:ring-primary-500 focus:border-primary-500 text-base bg-white shadow-sm touch-target-min"
            aria-label="Task Description"
          ></textarea>
        </div>

        <!-- Due Date Input -->
        <div>
          <label for="task-due-date" class="block text-sm font-medium text-gray-700 mb-1">Due Date (Optional)</label>
          <!-- Using native date input for best mobile UX and accessibility -->
          <input
            type="date"
            id="task-due-date"
            v-model="newTask.dueDate"
            class="block w-full px-3 py-3 rounded-lg border border-gray-300 focus:ring-primary-500 focus:border-primary-500 text-base bg-white shadow-sm touch-target-min"
            aria-label="Task Due Date"
          />
        </div>

        <!-- Priority Select -->
        <div>
          <label for="task-priority" class="block text-sm font-medium text-gray-700 mb-1">Priority</label>
          <select
            id="task-priority"
            v-model="newTask.priority"
            required
            class="block w-full px-3 py-3 rounded-lg border border-gray-300 focus:ring-primary-500 focus:border-primary-500 text-base bg-white shadow-sm touch-target-min"
            aria-required="true"
            aria-label="Task Priority"
          >
            <option value="low">🔵 Low</option>
            <option value="medium">🟡 Medium</option>
            <option value="high">🔴 High</option>
          </select>
        </div>

        <!-- Action Buttons -->
        <div class="flex justify-end gap-3 mt-4">
          <!-- Cancel Button -->
          <button
            type="button"
            @click="cancel"
            class="px-6 py-3 border border-gray-300 rounded-lg text-gray-700 font-semibold hover:bg-gray-50 transition-colors touch-target-min"
            aria-label="Cancel adding task"
          >
            Cancel
          </button>
          <!-- Save Button -->
          <button
            type="submit"
            class="px-6 py-3 bg-primary text-white rounded-lg font-semibold hover:bg-primary-hover transition-colors touch-target-min"
            aria-label="Save new task"
          >
            Save Task
          </button>
        </div>
      </form>
    </div>
  </div>
  <!-- ===== [New Feature] END ===== -->
</template>

<script setup lang="ts">
// ===== Imports & Config =====
import { ref, watch } from 'vue'; // ref for state, watch to reset form

// ===== Types & Interfaces =====
/**
 * Defines the structure for a new task object to be emitted.
 * Includes properties from the PRD: title, description, dueDate, priority.
 * Also includes the column it was added to.
 */
interface NewTaskData {
  title: string;
  description: string;
  dueDate: string; // Using string for native date input value (YYYY-MM-DD)
  priority: 'low' | 'medium' | 'high';
  column: string; // The board ID where the task was added
}

/**
 * Defines the props accepted by the AddTaskModal component.
 * - modelValue: Controls the visibility of the modal (for v-model).
 * - initialColumn: The ID of the board from which the modal was opened.
 */
interface Props {
  modelValue: boolean; // Controls modal visibility
  initialColumn: string; // The board ID where the task is being added
}

// ===== Props & Emits =====
// Define props with TypeScript types and default values
const props = withDefaults(defineProps<Props>(), {
  modelValue: false, // Modal is hidden by default
  initialColumn: 'masterlist', // Default to masterlist if not specified
});

// Define the events emitted by the AddTaskModal component.
// - update:modelValue: Emitted to close the modal (for v-model).
// - saveTask: Emitted when the form is submitted successfully, includes the new task data.
const emit = defineEmits<{
  (e: 'update:modelValue', value: boolean): void;
  (e: 'saveTask', task: NewTaskData): void;
}>();

// ===== State =====
/**
 * Reactive state for the new task form inputs.
 * Initialized with default values.
 */
const newTask = ref<NewTaskData>({
  title: '',
  description: '',
  dueDate: '', // Native date input uses YYYY-MM-DD string
  priority: 'low', // Default priority
  column: props.initialColumn, // Set initial column from prop
});

// ===== Watchers =====
/**
 * Watch for changes in the modal's visibility (modelValue).
 * When the modal is opened (modelValue becomes true), reset the form
 * and set the correct initial column.
 */
watch(() => props.modelValue, (isOpen) => {
  if (isOpen) {
    resetForm();
    newTask.value.column = props.initialColumn; // Ensure column is set correctly on open
  }
});

// ===== Methods =====
/**
 * Resets the form fields to their initial empty/default state.
 */
function resetForm() {
  newTask.value = {
    title: '',
    description: '',
    dueDate: '',
    priority: 'low',
    column: props.initialColumn, // Reset column based on the prop
  };
}

/**
 * Handles the form submission.
 * Emits the 'saveTask' event with the new task data.
 * Resets the form and closes the modal.
 */
function saveTask() {
  // Basic validation: check if title is not empty (required attribute handles HTML validation)
  if (!newTask.value.title.trim()) {
    // In a real app, show a user-friendly error message here
    console.error("Task title is required.");
    return; // Prevent saving if title is empty
  }

  // Emit the new task data to the parent component
  emit('saveTask', { ...newTask.value }); // Emit a copy to avoid direct state modification outside

  // Reset the form and close the modal
  resetForm();
  emit('update:modelValue', false); // Close the modal
}

/**
 * Handles the cancel button click.
 * Resets the form and closes the modal without saving.
 */
function cancel() {
  resetForm(); // Reset form state
  emit('update:modelValue', false); // Close the modal
}

</script>

<!--
  Tailwind CSS Styling Notes:
  - fixed inset-0: Positions the overlay to cover the entire viewport.
  - bg-black bg-opacity-50: Semi-transparent black background for the overlay.
  - flex items-center justify-center p-4 z-50: Centers the modal content, adds padding, ensures it's on top.
  - bg-white rounded-lg p-6 w-full max-w-sm md:max-w-md shadow-xl: Styles the modal content container.
  - flex flex-col max-h-[90vh] overflow-y-auto: Allows content to scroll if it exceeds 90% of viewport height.
  - text-xl font-semibold mb-4 text-gray-800: Styles the modal title.
  - flex flex-col gap-4: Stacks form elements vertically with spacing.
  - block text-sm font-medium text-gray-700 mb-1: Styles for input labels.
  - block w-full px-3 py-3 rounded-lg border border-gray-300 focus:ring-primary-500 focus:border-primary-500 text-base bg-white shadow-sm: Standard input styling.
  - touch-target-min: Custom utility class (defined in tailwind.config.js) applied to interactive elements (inputs, buttons) to ensure minimum touch size (48x48px). The padding on inputs contributes to this.
  - flex justify-end gap-3 mt-4: Styles the button container, aligning buttons to the right with spacing.
  - px-6 py-3 border border-gray-300 rounded-lg text-gray-700 font-semibold hover:bg-gray-50 transition-colors: Styling for the Cancel button.
  - px-6 py-3 bg-primary text-white rounded-lg font-semibold hover:bg-primary-hover transition-colors: Styling for the Save button, using Stratonea primary color.
  - v-if="modelValue": Conditionally renders the modal based on the prop, improving performance when hidden.
  - aria-* attributes: Added for accessibility (roles, labels, required status).
-->