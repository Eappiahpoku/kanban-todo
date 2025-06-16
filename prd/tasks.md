# ✅ Step-by-Step Task List: Kanban To-Do List App

---

## 🚧 Phase 1: Project Setup

| #  | Task |
|----|------|
| 1.1 | Create a new project folder and initialize with `npm init` (if needed). |
| 1.2 | Install Vue CLI: `npm install -g @vue/cli` (or use Vite for faster setup). |
| 1.3 | Scaffold a Vue project: `vue create kanban-todo-app` or `npm create vite@latest`. |
| 1.4 | Set up Git and initialize a GitHub repo. |
| 1.5 | Install Tailwind CSS (optional but recommended for styling). |

---

## 🧱 Phase 2: File & Component Structure

| #  | Task |
|----|------|
| 2.1 | Create the base components: `Header.vue`, `TaskCard.vue`, `TaskBoard.vue`, `AddTaskModal.vue`. |
| 2.2 | Create views/pages: `HomeView.vue` or `KanbanView.vue`. |
| 2.3 | Create utility files: `data.js` (for dummy tasks), `storage.js` (for localStorage). |
| 2.4 | Set up routing if planning for multiple pages (e.g., Vue Router). |

---

## 🖼️ Phase 3: UI & Layout

| #  | Task |
|----|------|
| 3.1 | Design the main layout: three columns labeled “My Day”, “My Week”, and “Masterlist”. |
| 3.2 | Style each column using Flexbox or Grid. |
| 3.3 | Add a responsive header with logo, add button, search, and settings. |
| 3.4 | Make the layout responsive (mobile & desktop). |

---

## 🎯 Phase 4: Task Functionality

| #  | Task |
|----|------|
| 4.1 | Create a data model for tasks (id, title, description, date, priority, completed, column). |
| 4.2 | Implement logic to add new tasks to the right column. |
| 4.3 | Build a modal or form for adding/editing a task. |
| 4.4 | Implement delete and edit functionality. |
| 4.5 | Implement mark-as-complete toggle with UI feedback (e.g., strikethrough). |
| 4.6 | Filter tasks to show only those in “My Day”, “My Week”, and “Masterlist”. |

---

## 🎯 Phase 5: Drag-and-Drop

| #  | Task |
|----|------|
| 5.1 | Install drag-and-drop library (`vue-draggable-next` or `vuedraggable`). |
| 5.2 | Make task cards draggable within and between columns. |
| 5.3 | Update the task's column when dropped in a new section. |
| 5.4 | Save task positions/order if needed. |

---

## 💾 Phase 6: Data Persistence

| #  | Task |
|----|------|
| 6.1 | Use `localStorage` to save tasks and retrieve them on load. |
| 6.2 | Wrap read/write logic in a `storage.js` utility file. |
| 6.3 | Test that tasks persist even after refreshing the browser. |

---

## 🔍 Phase 7: Optional Features

| #  | Task |
|----|------|
| 7.1 | Add a search bar to filter tasks by title. |
| 7.2 | Add priority tags (color-coded) to each task. |
| 7.3 | Add a calendar date picker for due dates. |
| 7.4 | Add a completed section (optional dropdown). |
| 7.5 | Add light/dark theme toggle. |

---

## 🚀 Phase 8: Deployment

| #  | Task |
|----|------|
| 8.1 | Clean up unused code and console logs. |
| 8.2 | Test on multiple screen sizes. |
| 8.3 | Push final project to GitHub. |
| 8.4 | Deploy using Vercel, Netlify, or GitHub Pages. |
| 8.5 | Share the live link! |

---

## 🧪 Bonus: Testing & Review

| #  | Task |
|----|------|
| 9.1 | Manually test all features: Add, Edit, Move, Delete, Complete. |
| 9.2 | Ask friends or classmates to test and give feedback. |
| 9.3 | Write a short README explaining the app. |
