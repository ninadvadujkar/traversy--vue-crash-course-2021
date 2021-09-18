<template>
  <div>
    <AddTask v-if="showAddTask" @add-task="addTask" />
    <Tasks
      :tasks="tasks"
      @delete-task="deleteTask"
      @toggle-reminder="toggleReminder"
    />
  </div>
</template>

<script>
import AddTask from '../components/AddTask.vue';
import Tasks from '../components/Tasks.vue';

export default {
  name: "Home",
  components: {
    AddTask,
    Tasks
  },
  props: {
    showAddTask: Boolean
  },
  data() {
    return {
      tasks: []
    }
  },
  methods: {
    async addTask(newTask) {
      const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(newTask)
      });
      const data = await res.json();
      this.tasks = [...this.tasks, data];
    },
    async deleteTask(id) {
      if (confirm('Are you sure?')) {
        const res = await fetch(`api/tasks/${id}`, {
          method: 'DELETE',
        });
        res.status === 200 ?
          this.tasks = this.tasks.filter(task => task.id !== id) :
          alert('Error in deleting task');
      }
    },
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id);
      const updTask = {...taskToToggle, reminder: !taskToToggle.reminder };
      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(updTask)
      });
      const data = await res.json();

      this.tasks = this.tasks.map(task => {
        if (task.id === id) {
          return { ...task, reminder: data.reminder }
        }
        return task;
      });
    },
    async fetchTasks() {
      const res = await fetch('api/tasks');
      const data = await res.json();
      return data;
    },
    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`);
      const data = await res.json();
      return data;
    }
  },
  // created lifecycle method
  async created() {
    this.tasks = await this.fetchTasks();
  }
};
</script>