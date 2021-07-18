<template>
  <h1>Todo</h1>
  <label>Task Label: </label>
  <input type="text" />
  <br />
  <label>Task Type: </label>
  <selector>
    <option v-for="(type, index) in newTask" :key="index">{{type}}</option>
  </selector>
  <button class="button" @click="addTask">Add Task</button>

  <br />
  <h1>Task List</h1>
  <button class="button">All</button> |
  <button class="button">Incomplete</button> |
  <button class="button">Complete</button>
  <br />
  <div>{{ filterTasks }}</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import { TodoItem } from '../types'

export default defineComponent({
  data: () => ({
    newTask: {
      label: '',
      type: 'personal'
    } as TodoItem,
    taskItems: [] as TodoItem[],
    listFilter: 'all'
  }),
  computed: {
    filterTasks(): TodoItem[] {
      if (this.listFilter === 'complete') {
        return this.taskItems.filter(
          (item: TodoItem) => item.isComplete === true
        )
      } else if (this.listFilter === 'incomplete') {
        return this.taskItems.filter(
          (item: TodoItem) => item.isComplete === false
        )
      } else {
        return this.taskItems
      }
    }
  },
  methods: {
    addTask() {
      this.taskItems.push({
        ...this.newTask,
        isComplete: false
      })
    }
  }
})
</script>
