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

<template>
  <div class="todo-page">
    <h1>Todo</h1>
    <form @submit.prevent>
      <div class="mb1">
        <label for="task-label">Task Label: </label>
        <input type="text" id="task-label" v-model="newTask.label" />
      </div>
      <div class="mb1">
        <label for="task-type">Task Type: </label>
        <select name="task-type" id="task-type" v-model="newTask.type">
          <option value="personal">person</option>
          <option value="work">work</option>
          <option value="miscellaneous">miscellaneous</option>
        </select>
      </div>
      <button @click="addTask">Add Task</button>
    </form>
    <h1>Task List</h1>
    <div>
      <button @click="listFilter = 'all'">all</button> |
      <button @click="listFilter = 'incomplete'">incomplete</button> |
      <button @click="listFilter = 'complete'">complete</button>
    </div>
    <ul>
      <li v-for="(task, index) in filterTasks" :key="`task-${index}`">
        <input type="checkbox" v-model="task.isComplete" />
        {{ task.label }}
        ({{ task.type }})
      </li>
    </ul>
  </div>
</template>

<style>
.todo-page {
  width: 800px;
  margin: 0 auto;
}
ul {
  list-style: none;
}
.mb1 {
  margin-bottom: 1rem;
}
</style>
