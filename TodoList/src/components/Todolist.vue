<template>
  <div class="todo-container">
    <div class="todo-card">
      <!-- Header -->
      <header class="todo-header">
        <h1>To Do List</h1>
      </header>

      <!-- Main Content -->
      <main class="todo-body">
        <!-- Error Message -->
        <p v-if="error" class="error-message">{{ error }}</p>

        <!-- Add Task -->
        <div class="todo-input-container">
          <input
            v-model="newTaskText"
            @keyup.enter="handleAddTask"
            placeholder="e.g. make a video"
            class="todo-input"
            :disabled="isCreating"
          />
          <button
            @click="handleAddTask"
            :disabled="!newTaskText.trim() || isCreating"
            class="add-button"
          >
            <svg class="add-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" d="M12 4v16m8-8H4" />
            </svg>
          </button>
        </div>

        <!-- Loading -->
        <div v-if="isLoading" class="loading">
          <div class="loading-spinner"></div>
          <p>Loading tasks...</p>
        </div>

        <!-- Task List -->
        <div v-else-if="tasks.length" class="task-list">
          <div v-for="task in tasks" :key="task.id" class="task-item">
            <!-- Checkbox -->
            <button
              @click="toggleTask(task)"
              :class="['task-checkbox', { completed: task.completed }]"
            >
              <svg class="check-icon" fill="currentColor" viewBox="0 0 20 20">
                <path fill-rule="evenodd"
                  d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"
                  clip-rule="evenodd" />
              </svg>
            </button>

            <!-- Task Text -->
            <div class="task-text-container" style="flex: 1;">
              <input
                v-if="editingId === task.id"
                v-model="editText"
                @keyup.enter="saveEdit(task.id)"
                @keyup.escape="cancelEdit"
                @blur="saveEdit(task.id)"
                class="task-input"
                ref="editInput"
              />
              <span
                v-else
                :class="['task-text', { completed: task.completed }]"
                @dblclick="startEdit(task)"
              >
                {{ task.text }}
              </span>
            </div>

            <!-- Actions -->
            <div class="task-actions">
              <button
                v-if="editingId !== task.id"
                @click="startEdit(task)"
                class="action-button edit-button"
              >
                <svg class="action-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414
                       a2 2 0 112.828 2.828L11.828 15H9v-2.828
                       l8.586-8.586z" />
                </svg>
              </button>
              <button @click="deleteTask(task.id)" class="action-button delete-button">
                <svg class="action-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="M19 7l-.867 12.142A2 2 0 0116.138
                       21H7.862a2 2 0 01-1.995-1.858L5
                       7m5 4v6m4-6v6m1-10V4a1 1 0
                       00-1-1h-4a1 1 0 00-1
                       1v3M4 7h16" />
                </svg>
              </button>
            </div>
          </div>
        </div>

        <!-- Empty State -->
        <div v-else class="empty-state">
          <svg class="empty-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1"
              d="M9 12h6m-6 4h6m2 5H7a2 2
                 0 01-2-2V5a2 2 0 012-2h5.586a1
                 1 0 01.707.293l5.414 5.414a1
                 1 0 01.293.707V19a2 2 0 01-2 2z" />
          </svg>
          <p>No tasks yet. Add one above to get started!</p>
        </div>

        <!-- Footer -->
        <footer v-if="tasks.length" class="footer">
          <span class="task-count">{{ completedCount }} of {{ tasks.length }} completed</span>
          <button v-if="completedCount" @click="clearCompleted" class="clear-button">
            Clear completed
          </button>
        </footer>
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, nextTick } from 'vue'

interface Task {
  id: string
  text: string
  completed: boolean
  createdAt: string
}

// State
const tasks = ref<Task[]>([])
const newTaskText = ref('')
const editingId = ref<string | null>(null)
const editText = ref('')
const isLoading = ref(false)
const isCreating = ref(false)
const error = ref('')
const editInput = ref<HTMLInputElement>()

// Computed
const completedCount = computed(() => tasks.value.filter(t => t.completed).length)

// ===== LocalStorage Helpers =====
const loadFromStorage = () => {
  const saved = localStorage.getItem('tasks')
  tasks.value = saved ? JSON.parse(saved) : []
}

const saveToStorage = () => {
  localStorage.setItem('tasks', JSON.stringify(tasks.value))
}

// Load tasks
const loadTasks = () => {
  try {
    isLoading.value = true
    loadFromStorage()
  } catch {
    error.value = 'Failed to load tasks'
  } finally {
    isLoading.value = false
  }
}

// Add Task
const handleAddTask = () => {
  const text = newTaskText.value.trim()
  if (!validateTaskText(text)) return
  if (isDuplicate(text)) {
    error.value = 'This task already exists'
    return
  }
  try {
    isCreating.value = true
    const task: Task = {
      id: Date.now().toString(),
      text,
      completed: false,
      createdAt: new Date().toISOString()
    }
    tasks.value.unshift(task)
    saveToStorage()
    newTaskText.value = ''
  } catch {
    error.value = 'Failed to create task'
  } finally {
    isCreating.value = false
  }
}

// Toggle Task
const toggleTask = (task: Task) => {
  task.completed = !task.completed
  saveToStorage()
}

// Start Edit
const startEdit = async (task: Task) => {
  editingId.value = task.id
  editText.value = task.text
  await nextTick()
  editInput.value?.focus()
  editInput.value?.select()
}

// Save Edit
const saveEdit = (id: string) => {
  const text = editText.value.trim()
  if (!validateTaskText(text)) return
  if (isDuplicate(text, id)) {
    error.value = 'This task already exists'
    return
  }
  const index = tasks.value.findIndex(t => t.id === id)
  if (index !== -1) {
    tasks.value[index].text = text
    saveToStorage()
  }
  cancelEdit()
}

// Cancel Edit
const cancelEdit = () => {
  editingId.value = null
  editText.value = ''
}

// Delete Task
const deleteTask = (id: string) => {
  if (!confirm('Delete this task?')) return
  tasks.value = tasks.value.filter(t => t.id !== id)
  saveToStorage()
}

// Clear Completed
const clearCompleted = () => {
  tasks.value = tasks.value.filter(t => !t.completed)
  saveToStorage()
}

// Helpers
const validateTaskText = (text: string) => {
  if (!text) {
    error.value = 'Please enter a task description'
    return false
  }
  if (text.length > 100) {
    error.value = 'Task description must be less than 100 characters'
    return false
  }
  error.value = ''
  return true
}

const isDuplicate = (text: string, excludeId?: string) =>
  tasks.value.some(t => t.id !== excludeId && t.text.toLowerCase() === text.toLowerCase())

// Init
onMounted(loadTasks)
</script>
