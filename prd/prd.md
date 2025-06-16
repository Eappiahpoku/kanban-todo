# 📌 Project Requirements Document (PRD)  

## 📝 Project Name: Kanban To-Do List App

---

## 🎯 Purpose

To build a responsive and user-friendly Kanban-style To-Do list app that helps users visually organize their tasks into three sections: **My Day**, **My Week**, and **Masterlist**.

---

## 💡 Key Features

| Feature              | Description |
|----------------------|-------------|
| **Three Kanban Boards** | Users can manage tasks across **My Day**, **My Week**, and **Masterlist** columns. |
| **Drag & Drop Tasks**   | Tasks can be dragged between the three boards. |
| **Create Task**         | Users can add new tasks with a title, description, due date, and priority. |
| **Edit/Delete Task**    | Tasks can be updated or removed at any time. |
| **Mark as Completed**   | Users can mark tasks as complete; completed tasks are visually distinct. |
| **Persistent Storage**  | Tasks are saved in local storage or backend to persist across sessions. |
| **Responsive Design**   | Fully functional on both desktop and mobile devices. |
| **Search & Filter**     | Tasks can be filtered by keyword, priority, or due date. |

---

## 🧱 Functional Requirements

| ID | Requirement |
|----|-------------|
| FR1 | The app must allow users to create a new task with a title and optional description. |
| FR2 | The app must allow users to drag tasks between **My Day**, **My Week**, and **Masterlist**. |
| FR3 | The app must allow users to edit task details. |
| FR4 | The app must allow users to delete tasks. |
| FR5 | The app must show completed tasks with a different style (e.g., strikethrough or faded). |
| FR6 | The app must persist task data using Local Storage or a backend database. |
| FR7 | The app must work on mobile, tablet, and desktop screens. |
| FR8 | The app must allow sorting and filtering tasks by date, priority, or status. |

---

## 🛠️ Technical Requirements

| ID | Requirement |
|----|-------------|
| TR1 | Use **Vue.js** or **React** as the frontend framework. |
| TR2 | Use **HTML5**, **CSS3**, and **JavaScript** (or TypeScript). |
| TR3 | Implement drag-and-drop using a library like `Vue.Draggable` or `react-beautiful-dnd`. |
| TR4 | Use **LocalStorage** initially for data persistence; plan for optional backend API later. |
| TR5 | Apply mobile-first responsive design principles. |
| TR6 | Use component-based architecture for TaskItem, TaskBoard, Header, etc. |

---

## 🧪 Non-Functional Requirements

| Requirement | Description |
|-------------|-------------|
| **Performance** | App should load and respond quickly with minimal delays. |
| **Scalability** | Design architecture to allow adding more Kanban boards in the future. |
| **Accessibility** | Use semantic HTML and ARIA tags where necessary. |
| **Usability** | Ensure UI is intuitive, clean, and beginner-friendly. |

---

## 📋 User Stories

| As a... | I want to... | So that... |
|---------|--------------|------------|
| User | Create a task in **My Day** | I can plan my daily goals |
| User | Move a task to **My Week** | I can track tasks over a longer time span |
| User | Add tasks to **Masterlist** | I can keep a full backlog of all tasks |
| User | Drag a task between boards | I can reprioritize quickly |
| User | Mark a task as complete | I can track my progress |

---

## 🖼️ UI Layout (Wireframe Overview)

- **Header**: App logo and theme switcher (optional)
- **Main Section**:
  - 3 Columns:
    - **My Day**
    - **My Week**
    - **Masterlist**
  - Each column shows task cards
  - New task button at the top of each column
- **Task Card**:
  - Title
  - Due date
  - Priority tag
  - Edit/Delete buttons
  - Mark as complete checkbox

---

## 🗂️ Optional Enhancements

| Feature | Description |
|---------|-------------|
| Notifications | Alert user about upcoming or overdue tasks |
| User Authentication | Allow sign-in with email or Google account |
| Dark Mode | Toggle between light and dark themes |
| Task Labels | Add color-coded tags to categorize tasks |
| Calendar View | View tasks in a calendar format |

---

## 📅 Milestones

| Phase | Description | Target Date |
|-------|-------------|-------------|
| Planning | Finalize features, wireframes, and tech stack | [Insert Date] |
| Design | UI/UX design and prototyping | [Insert Date] |
| Development | Code main features and Kanban logic | [Insert Date] |
| Testing | Perform bug fixes and UX testing | [Insert Date] |
| Launch | Deploy to web hosting platform | [Insert Date] |

---

## 🧰 Tools & Stack

| Area | Technology |
|------|------------|
| Frontend Framework | Vue.js / React |
| Styling | Tailwind CSS / SCSS |
| Drag and Drop | Vue.Draggable / react-beautiful-dnd |
| Storage | LocalStorage or Firebase (optional) |
| Version Control | Git & GitHub |
| Hosting | Netlify / Vercel / GitHub Pages |

---

## ✅ Success Criteria

- Users can create, edit, delete, and move tasks.
- Tasks persist between sessions.
- App is mobile and desktop-friendly.
- Users can track daily, weekly, and long-term tasks visually.

---
