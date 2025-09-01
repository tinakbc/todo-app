<template>
  <div class="list-wrapper">
    <HeaderTitle />
    <div class="input-wrapper">
      <form class="form" @submit.prevent="addTask">
        <button id="btnAdd" type="submit">
          <i class="fa-regular fa-circle"></i>
          <i class="fas fa-plus"></i>
        </button>
        <input
          id="inputList"
          type="text"
          placeholder="Add a new to-do"
          v-model="newTask"
        />
      </form>
    </div>

    <ul id="list">
      <TaskItem
        v-for="(task, index) in tasks"
        :key="index"
        :task="task"
        @update-task="updateTask(index, $event)"
        @delete-task="deleteTask(index)"
      />
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'
import TaskItem from './components/TaskItem.vue'
import HeaderTitle from './components/HeaderTitle.vue'

const newTask = ref('')
const tasks = ref([])

onMounted(() => {
  const saved = localStorage.getItem('tasks')
  if (saved) {
    tasks.value = JSON.parse(saved)
  }
})

watch(tasks, saveTasks, { deep: true })

function addTask() {
  if (newTask.value.trim() === '') return

  tasks.value.push({
    text: newTask.value.trim(),
    completed: false,
    editing: false
  })

  newTask.value = ''
}

function deleteTask(index) {
  tasks.value.splice(index, 1)
}

function updateTask(index, updatedTask) {
  tasks.value[index] = { ...updatedTask }
}

function saveTasks() {
  localStorage.setItem('tasks', JSON.stringify(tasks.value))
}
</script>

<style src="./assets/stylesheet.css"></style>