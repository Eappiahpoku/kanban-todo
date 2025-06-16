<!--
  KanbanView.vue
  Main Kanban board view for Stratonea To-Do app.
  - Manages all task state, board filtering, and modal logic.
  - Ghana mobile-first, offline-first, and fully documented for learning.
  - Follows Stratonea coding and documentation standards.
-->

<template>
  <!-- ===== App Layout START ===== -->
  <div class="flex flex-col h-screen bg-gray-50" aria-label="Kanban Board Application View" role="main">
    <!-- App Header -->
    <Header />

    <!-- Main Content: Boards and Search -->
    <div class="flex flex-col md:flex-row flex-1 overflow-hidden p-4">
      <!-- Search Bar -->
      <div class="mb-4 md:mb-0 md:mr-4 w-full md:w-auto flex-shrink-0">
        <SearchBar v-model="searchQuery" />
      </div>
      <!-- Kanban Boards -->
      <div class="flex flex-row md:flex-wrap flex-1 overflow-x-auto md:overflow-x-hidden gap-4 pb-4 md:pb-0">
        <!-- ===== [New Feature] START: Add @completeTask event handler ===== -->
        <TaskBoard
          title="My Day"
          board-id="my-day"
          v-model="myDayTasks"
          @addTask="(id) => handleAddTask(id as BoardId)"
          @completeTask="handleCompleteTask"
          class="flex-shrink-0 w-[300px] md:w-1/3"
        />
        <TaskBoard
          title="My Week"
          board-id="my-week"
          v-model="myWeekTasks"
          @addTask="(id) => handleAddTask(id as BoardId)"
          @completeTask="handleCompleteTask"
          class="flex-shrink-0 w-[300px] md:w-1/3"
        />
        <TaskBoard
          title="Masterlist"
          board-id="masterlist"
          v-model="masterlistTasks"
          @addTask="(id) => handleAddTask(id as BoardId)"
          @completeTask="handleCompleteTask"
          class="flex-shrink-0 w-[300px] md:w-1/3"
        />
        <!-- ===== [New Feature] END ===== -->
      </div>
    </div>
    <!-- Completed Tasks List -->
    <div class="p-4 pt-0">
      <CompletedList :completed-tasks="completedTasks" />
    </div>
    <!-- Add Task Modal -->
    <AddTaskModal
      v-model="isModalOpen"
      :initial-column="columnToAdd"
      @saveTask="handleSaveTask"
    />
  </div>
  <!-- ===== App Layout END ===== -->
</template>

<script setup lang="ts">
// ===== File-Level Documentation =====
/**
 * KanbanView.vue
 * - Main view for the Stratonea Kanban To-Do app.
 * - Handles all task state, board filtering, modal logic, and search.
 * - Ghana mobile-first, offline-first, and fully documented for learning.
 */

// ===== Imports & Config =====
import { ref, computed } from 'vue'
import { v4 as uuidv4 } from 'uuid'
import Header from '@/components/layout/Header.vue'
import TaskBoard from '@/components/layout/TaskBoard.vue'
import SearchBar from '@/components/layout/SearchBar.vue'
import AddTaskModal from '@/components/layout/AddTaskModal.vue'
import CompletedList from '@/components/layout/CompletedList.vue'

// ===== Types & Interfaces =====
/**
 * BoardId: Allowed board columns for tasks.
 */
type BoardId = 'my-day' | 'my-week' | 'masterlist'

/**
 * Task: Represents a single to-do item.
 * - dueDate is always a string ('' if not set), never undefined.
 */
interface Task {
  id: string
  title: string
  description: string
  dueDate?: string // Always string, never undefined (see mock data)
  priority: 'low' | 'medium' | 'high'
  column: BoardId
  completed: boolean
}

/**
 * NewTaskData: Data structure from AddTaskModal.
 */
interface NewTaskData {
  title: string
  description: string
  dueDate: string // Always string ('' if not set)
  priority: 'low' | 'medium' | 'high'
  column: string
}

// ===== State =====
const allTasks = ref<Task[]>([])
const isModalOpen = ref(false)
const columnToAdd = ref<BoardId>('masterlist')
const searchQuery = ref('')

// ===== Computed Properties =====
const filteredTasks = computed(() => {
  const query = searchQuery.value.toLowerCase()
  if (!query) return allTasks.value
  return allTasks.value.filter(task =>
    task.title.toLowerCase().includes(query) ||
    task.description.toLowerCase().includes(query)
  )
})

const myDayTasks = computed({
  get: () => filteredTasks.value.filter(task => task.column === 'my-day' && !task.completed),
  set: (updatedList: Task[]) => {
    const others = allTasks.value.filter(task => task.column !== 'my-day' || task.completed)
    allTasks.value = [...others, ...updatedList]
  }
})

const myWeekTasks = computed({
  get: () => filteredTasks.value.filter(task => task.column === 'my-week' && !task.completed),
  set: (updatedList: Task[]) => {
    const others = allTasks.value.filter(task => task.column !== 'my-week' || task.completed)
    allTasks.value = [...others, ...updatedList]
  }
})

const masterlistTasks = computed({
  get: () => filteredTasks.value.filter(task => task.column === 'masterlist' && !task.completed),
  set: (updatedList: Task[]) => {
    const others = allTasks.value.filter(task => task.column !== 'masterlist' || task.completed)
    allTasks.value = [...others, ...updatedList]
  }
})

const completedTasks = computed(() =>
  filteredTasks.value.filter(task => task.completed)
)

// ===== Main Logic =====
/**
 * handleAddTask: Opens modal for adding a task to a board.
 * @param boardId - Which board to add to
 */
function handleAddTask(boardId: BoardId) {
  columnToAdd.value = boardId
  isModalOpen.value = true
}

/**
 * handleSaveTask: Adds a new task from modal data.
 * @param newTaskData - Data from AddTaskModal
 */
function handleSaveTask(newTaskData: NewTaskData) {
  // Always use '' for dueDate if not set, never undefined
  const newTask: Task = {
    id: uuidv4(),
    title: newTaskData.title,
    description: newTaskData.description,
    dueDate: newTaskData.dueDate || '',
    priority: newTaskData.priority,
    column: newTaskData.column as BoardId,
    completed: false
  }
  allTasks.value.push(newTask)
  // Modal closes automatically via v-model
}

/**
 * handleCompleteTask: Marks a task as completed.
 * @param taskId - Task ID to mark as completed
 */
function handleCompleteTask(taskId: string) {
  // ===== [New Feature] START =====
  // Find the task by ID and set completed to true
  const idx = allTasks.value.findIndex(task => task.id === taskId)
  if (idx !== -1) {
    allTasks.value[idx].completed = true
  }
  // ===== [New Feature] END =====
}

// ===== Mock Data for Learning =====
allTasks.value = [
  {
    id: uuidv4(),
    title: 'Plan project structure',
    description: 'Outline components and data flow',
    dueDate: '2025-06-15',
    priority: 'high',
    column: 'my-day',
    completed: false
  },
  {
    id: uuidv4(),
    title: 'Install dependencies',
    description: 'npm install vue, tailwind, uuid, vuedraggable',
    dueDate: '',
    priority: 'high',
    column: 'my-day',
    completed: false
  },
  {
    id: uuidv4(),
    title: 'Build Header component',
    description: 'Implement Stratonea header guidelines',
    dueDate: '',
    priority: 'medium',
    column: 'my-day',
    completed: false
  },
  {
    id: uuidv4(),
    title: 'Research offline storage options',
    description: 'Look into localforage, IndexedDB, PouchDB',
    dueDate: '2025-06-20',
    priority: 'medium',
    column: 'my-week',
    completed: false
  },
  {
    id: uuidv4(),
    title: 'Design database schema',
    description: 'Plan tables for tasks, users, etc.',
    dueDate: '2025-06-22',
    priority: 'high',
    column: 'my-week',
    completed: false
  },
  {
    id: uuidv4(),
    title: 'Set up backend API',
    description: 'Choose framework (Node/Express, Python/Flask), define endpoints',
    dueDate: '2025-06-25',
    priority: 'low',
    column: 'masterlist',
    completed: false
  },
  {
    id: uuidv4(),
    title: 'Implement user authentication',
    description: 'Sign up, login, logout',
    dueDate: '2025-06-30',
    priority: 'high',
    column: 'masterlist',
    completed: false
  },
  {
    id: uuidv4(),
    title: 'Write unit tests for components',
    description: 'Test Header, TaskBoard, etc.',
    dueDate: '',
    priority: 'medium',
    column: 'masterlist',
    completed: false
  },
  {
    id: uuidv4(),
    title: 'Completed: Old task example',
    description: 'This task was finished earlier.',
    dueDate: '2025-06-10',
    priority: 'low',
    column: 'masterlist',
    completed: true
  }
]

</script>

<!--
  All styling via Tailwind utility classes for maintainability and mobile-first workflow.
  See Stratonea documentation for further Ghana-specific optimizations.
-->