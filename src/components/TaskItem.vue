<template>
    <li>
      <input
        type="checkbox"
        class="checkbox"
        v-model="localTask.completed"
        @change="emitUpdate"
        :disabled="localTask.editing"
      />
  
      <span
        v-if="!localTask.editing"
        :style="{ textDecoration: localTask.completed ? 'line-through' : 'none' }"
      >
        {{ localTask.text }}
      </span>
  
      <input
        v-else
        v-model="localTask.text"
        class="newInputField"
        @keydown.enter="stopEditing"
      />
  
      <span class="edit-task" @click="startEditing" v-if="!localTask.completed">
        <i class="fa-solid fa-pencil"></i>
      </span>
      <span class="del-task" @click="$emit('delete-task')">
        <i class="fa-solid fa-x"></i>
      </span>
    </li>
  </template>
  
  <script setup>
  import { reactive, toRefs, watch } from 'vue'
  
  // Props
  const props = defineProps({
    task: Object
  })
  
  // Emit events
  const emit = defineEmits(['update-task', 'delete-task'])
  
  // Make local copy of task to allow editing
  const localTask = reactive({ ...props.task })
  
  // Emit updated task whenever it changes
  watch(localTask, () => {
    emit('update-task', localTask)
  }, { deep: true })
  
  function startEditing() {
    localTask.editing = true
  }
  
  function stopEditing() {
  if (localTask.text.trim() === '') {
    emit('delete-task')
  } else {
    localTask.editing = false
  }
}
  </script>