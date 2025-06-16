<!--
  TaskBoard.vue
  Represents a single Kanban board column (My Day, My Week, Masterlist).
  - Displays tasks relevant to this board.
  - Implements drag-and-drop using vuedraggable.
  - Provides an "+ Add Task" button.
  - Follows Stratonea guidelines (mobile-first, Tailwind, TypeScript).
-->

<template>
  <!-- Board Container -->
  <div
    class="flex flex-col w-full md:w-1/3 p-4 bg-gray-100 rounded-lg shadow-inner"
    :aria-label="`${title} Kanban Board`"
    role="region"
  >
    <!-- Board Title -->
    <h2 class="text-xl font-semibold mb-4 text-gray-800" :aria-label="`${title} column title`">
      {{ title }}
    </h2>

    <!-- Draggable Task List Area -->
    <draggable
      :modelValue="modelValue"
      @update:modelValue="onTasksChange"
      group="kanban-tasks"
      @change="onDragChange"
      item-key="id"
      class="task-list flex-1 overflow-y-auto min-h-[100px]"
      :aria-label="`List of tasks in ${title}`"
      role="list"
    >
      <template #item="{ element }: { element: Task }">
        <div
          class="task-placeholder p-3 mb-2 bg-white rounded shadow cursor-move flex items-start gap-3"
          :aria-label="`Task: ${element.title}`"
          role="listitem"
        >
          <!-- Mark as Completed Checkbox -->
          <input
            type="checkbox"
            class="mr-2 w-6 h-6 accent-green-600 mt-1"
            :checked="!!element.completed"
            @change="() => emit('completeTask', element.id)"
            aria-label="Mark as completed"
          />
          <!-- ===== [New Feature] START: Task Details Card ===== -->
          <div class="flex-1 min-w-0">
            <!-- Task Title -->
            <div class="font-medium text-base text-gray-900 truncate">
              {{ element.title }}
            </div>
            <!-- Priority & Due Date Row -->
            <div class="flex items-center gap-3 mt-1">
              <!-- Priority Dot & Label -->
              <span class="flex items-center gap-1 text-sm">
                <span
                  :class="[
                    'inline-block w-3 h-3 rounded-full',
                    element.priority === 'high' ? 'bg-red-500' :
                    element.priority === 'medium' ? 'bg-yellow-400' :
                    'bg-blue-500'
                  ]"
                  aria-label="Priority"
                ></span>
                <span class="capitalize text-gray-700">{{ element.priority }}</span>
              </span>
              <!-- Due Date -->
              <span v-if="element.dueDate" class="text-xs text-gray-500">
                • Due: {{ formatDueDate(element.dueDate) }}
              </span>
            </div>
          </div>
          <!-- ===== [New Feature] END ===== -->
        </div>
      </template>
    </draggable>

    <!-- Add Task Button -->
    <button
      @click="emit('addTask', boardId)"
      class="mt-4 p-3 bg-secondary text-white rounded-lg font-bold text-center touch-target-min hover:bg-secondary-hover transition-colors"
      aria-label="Add new task to this board"
    >
      + Add Task
    </button>
  </div>
</template>

<script setup lang="ts">
// ===== Imports & Config =====
import draggable from 'vuedraggable'
import { watch } from 'vue'

// ===== Types & Interfaces =====
interface Task {
  id: string
  title: string
  column: string // 'my-day', 'my-week', 'masterlist'
  completed?: boolean
  priority: 'low' | 'medium' | 'high' // ===== [New Feature] START =====
  dueDate?: string // Always string, never undefined
  // ===== [New Feature] END =====
}

interface Props {
  modelValue: Task[]
  title: string
  boardId: string
}

const props = withDefaults(defineProps<Props>(), {
  modelValue: () => [],
})

const emit = defineEmits<{
  (e: 'update:modelValue', value: Task[]): void
  (e: 'addTask', boardId: string): void
  (e: 'completeTask', taskId: string): void
  (e: 'moveTask', payload: { task: Task; toBoard: string; fromBoard: string }): void
}>()

// ===== Main Logic =====
/**
 * Handles changes to the order or content of tasks in this board.
 * Ensures all tasks in this board have the correct column.
 */
function onTasksChange(newTasks: Task[]) {
  newTasks.forEach(task => {
    if (task.column !== props.boardId) {
      task.column = props.boardId
    }
  })
  emit('update:modelValue', newTasks)
}

/**
 * Handles cross-board drag-and-drop events.
 * Emits a moveTask event to the parent with the moved task and new board.
 */
function onDragChange(evt: any) {
  // evt.added exists if a task was added to this board
  if (evt.added && evt.added.element) {
    emit('moveTask', {
      task: evt.added.element,
      toBoard: props.boardId,
      fromBoard: evt.added.element.column,
    })
  }
}

// ===== Helper Functions =====
/**
 * Formats the due date for display.
 * @param dueDate - string (YYYY-MM-DD or similar)
 * @returns string in readable format or 'N/A'
 */
function formatDueDate(dueDate?: string): string {
  if (!dueDate) return 'N/A'
  // Simple formatting: show as-is, or you can use Intl.DateTimeFormat for better UX
  return dueDate
}

// ===== Watchers =====
watch(
  () => props.modelValue,
  (newTasks, oldTasks) => {
    const addedTasks = newTasks.filter(
      newTask => !oldTasks.some(oldTask => oldTask.id === newTask.id)
    )
    addedTasks.forEach(task => {
      if (task.column !== props.boardId) {
        task.column = props.boardId
      }
    })
  },
  { deep: true }
)
</script>

<!--
  Tailwind CSS Styling Notes:
  - flex flex-col: Stacks title, task list, and button vertically.
  - w-full md:w-1/3: Full width on mobile, roughly one-third width on medium screens and up.
  - p-4: Padding around the board content.
  - bg-gray-100 rounded-lg shadow-inner: Background color, rounded corners, and subtle shadow.
  - text-xl font-semibold mb-4 text-gray-800: Styling for the board title.
  - task-list flex-1 overflow-y-auto: Allows the task list to take up available space and scroll if needed.
  - min-h-[100px]: Ensures the drag area is visible even when empty.
  - task-placeholder: Temporary styling for the task div.
  - p-3 mb-2 bg-white rounded shadow cursor-move: Padding, margin-bottom, background, rounded corners, shadow, and cursor style for draggable items.
  - mt-4 p-3 bg-secondary text-white rounded-lg font-bold text-center: Styling for the Add Task button.
  - touch-target-min: Custom utility class for minimum touch size.
  - hover:bg-secondary-hover transition-colors: Hover effect for the button.
-->