<template>
    <li :class="listClass">
      {{ title }}
      <div id="check">
        <button
          @click="handleToggle"
          :class="btnClass"
        >{{ props.completed ? 'v' : 'x' }}</button>
        <button @click="$emit('remove')">-</button>
      </div>
    </li>
  </template>
  
  
  <script setup lang="ts">
  import { computed, defineProps, defineEmits } from 'vue'
  
  const props = defineProps<{ title: string, completed: boolean }>()
  const emit = defineEmits(['remove', 'toggle'])
  
  function handleToggle() {
    emit('toggle')
  }
  
  const btnClass = computed(() => (props.completed ? 'green-color' : 'red-color'))
  const listClass = computed(() => (props.completed ? 'green-color' : 'red-color'))
  </script>
  
  
  <style scoped>
  li {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
  }
  #check {
    margin-left: auto;
  }
  
  .green-color {
    background-color: green;
    color: white;
  }
  
  .red-color {
    background-color: red;
    color: white;
  }
  
  button {
    cursor: pointer;
    border: none;
    padding: 5px 10px;
    margin-right: 5px;
  }
  </style>
  
  