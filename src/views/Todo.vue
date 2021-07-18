<template>
  <div class="todo-page">
    <form @submit.prevent>
      <div class="mb1">
        <h1>Todo</h1>
        <label for="task-label">Task Label: </label>
        <input type="text" id="task-label" v-model="newTask.label" />
      </div>
      <div class="mb1">
        <label for="task-type">Task Type: </label>
        <select>
          <option value="personal">person</option>
          <option value="work">work</option>
          <option value="miscellaneous">miscellaneous</option>
        </select>
      </div>
      <button class="button" @click="addTask">Add Task</button>
    </form>
    <div class="mb1">
      <h1>Task List</h1>
      <button class="button" @class="listFilter = 'all'">All</button> |
      <button class="button" @class="listFilter = 'incomplete'">
        incomplete
      </button>
      |
      <button class="button" @class="listFilter = 'complete'">complete</button>
      <ul>
        <li v-for="(task, index) in filterTasks" :key="index">
          <input class="checkbox" v-model="filteredTasks.isComplete" />
          {{ task.label }}
          ({{ task.type }})
        </li>
      </ul>
    </div>
  </div>
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
