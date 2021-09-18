<template>
  <div>
    <div v-for="task in tasks" :key="task.id">
      <Task
        :task="task"
        @delete-task="onDelete"
        @toggle-reminder="$emit('toggle-reminder', task.id)"
      />
    </div>
  </div>
</template>

<script>
import Task from './Task.vue';

export default {
  name: 'Tasks',
  components: {
    Task
  },
  props: {
    tasks: Array
  },
  emits:['delete-task', 'toggle-reminder'],
  methods: {
    onDelete(id) {
      this.$emit('delete-task', id);
    }
  }
}
</script>