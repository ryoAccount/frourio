<template>
  <div class="container">
    <div>
      <logo />
      <h1 class="title">todo list</h1>
      <div>
        <form @submit.prevent="createTask">
          <input v-model="newLabel" class="textbox" type="text" />
          <input type="submit" class="button" value="ADD" />
        </form>
        <ul class="tasks">
          <li v-show="tasks.length !== 0">
            <span class="head">No</span>
            <span class="head">done</span>
            <span class="head">todo</span>
          </li>
          <li v-for="(task, index) in tasks" :key="task.id">
            <label>
              <span class="no">{{ index + 1 }}</span>
              <input
                type="checkbox"
                class="done"
                :checked="task.done"
                @change="toggleDone(task)"
              />
              <span class="task">{{ task.label }}</span>
            </label>
            <input
              type="button"
              class="button"
              value="DELETE"
              style="float: right"
              @click="deleteTask(task)"
            />
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import type { Task } from '$prisma/client'

export default Vue.extend({
  data() {
    return {
      tasks: [] as Task[],
      newLabel: ''
    }
  },
  async fetch() {
    await this.fetchTasks()
  },
  methods: {
    async fetchTasks() {
      this.tasks = await this.$api.tasks.$get()
    },
    async createTask() {
      if (!this.newLabel) return

      await this.$api.tasks.post({ body: { label: this.newLabel } })
      this.newLabel = ''
      await this.fetchTasks()
    },
    async toggleDone(task: Task) {
      await this.$api.tasks
        ._taskId(task.id)
        .patch({ body: { done: !task.done } })
      await this.fetchTasks()
    },
    async deleteTask(task: Task) {
      await this.$api.tasks._taskId(task.id).delete()
      await this.fetchTasks()
    }
  }
})
</script>

<style scoped>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 50px;
  color: darkslategrey;
  letter-spacing: 1px;
}

.textbox {
  border: 1.5px solid grey;
  border-radius: 5px;
  outline: none;
}

.textbox:focus {
  border: 2px solid grey;
}

.button {
  border: 1.5px solid grey;
  border-radius: 5px;
  background-color: white;
  color: black;
  outline: none;
}

.button:hover {
  pointer-events: visible;
}

.tasks {
  width: 400px;
  padding: 0;
  margin: 20px auto 0;
  list-style-type: none;
  text-align: left;
}

.tasks > li {
  margin-top: 15px;
}

.task {
  border-bottom: 2px solid lightgray;
}

.head {
  font-weight: 600;
  margin: 0 5px;
}

.no {
  margin-left: 10px;
}
.done {
  position: relative;
  top: 1px;
  margin-left: 30px;
  margin-right: 25px;
}
</style>
