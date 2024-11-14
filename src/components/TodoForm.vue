<template>
    <div>
      <h1>Mes tâches :</h1>
      <form @submit.prevent="addNewTask">
        <input v-model="newTaskText" placeholder="Ajouter une tâche" />
        <button type="submit">+</button>
      </form>
      <ul>
        <TodoList
          v-for="(task, index) in filteredTasks"
          :key="task.id"
          :title="task.title"
          :completed="task.completed"
          @remove="removeTask(index)"
          @toggle="toggleTaskCompletion(index)"
        />
      </ul>
      <div class="taskCheck">
        <label v-for="type in allTask" :key="type">
          <input
            type="radio"
            :value="type"
            v-model="selectedTask"
          />
          {{ type }}
        </label>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref, onMounted, watch, computed } from 'vue'
  import TodoList from './TodoList.vue'
  
  const newTaskText = ref('')
  const tasks = ref<{ id: number, title: string, completed: boolean }[]>([])
  const allTask = ["Toute", "Complète", "Incomplète"]
  const selectedTask = ref("Toute")
  
  let nextTaskId = 0
  
  function addNewTask() {
    if (newTaskText.value) {
      tasks.value.push({
        id: nextTaskId++,
        title: newTaskText.value,
        completed: false
      })
      newTaskText.value = ''
    }
  }
  
  function removeTask(index: number) {
    tasks.value.splice(index, 1)
  }
  
  function toggleTaskCompletion(index: number) {
    tasks.value[index].completed = !tasks.value[index].completed
  }

  const filteredTasks = computed(() => {
    if (selectedTask.value === "Complète") {
        return tasks.value.filter(task => task.completed)
    } else if (selectedTask.value === "Incomplète") {
        return tasks.value.filter(task => !task.completed)
    } else {
        return tasks.value
    }
    console.log(filteredTasks)
  })
  
  onMounted(() => {
    const storedTasks = localStorage.getItem('tasks')
    if (storedTasks) {
      try {
        tasks.value = JSON.parse(storedTasks)
        if (tasks.value.length > 0) {
          nextTaskId = tasks.value[tasks.value.length - 1].id + 1
        }
      } catch (e) {
        localStorage.removeItem('tasks')
      }
    }
  })
  
  watch(
    tasks,
    (newTasks) => {
      localStorage.setItem('tasks', JSON.stringify(newTasks))
    },
    { deep: true }
  )
  </script>  
  
  <style scoped>
  input {
    margin-right: 10px;
  }
  ul {
    padding: 0;
  }
  button {
    cursor: pointer;
  }
  </style>
  