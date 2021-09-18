<template>
  <div @dblclick="$emit('toggle-reminder')" :class="[task.reminder ? 'reminder' : '', 'task']">
    <h3>{{ task.text }}
      <button class="delete-task" @click="$emit('delete-task', task.id)">
        <i class="fas fa-times"></i>
      </button>
    </h3>
    <p>{{ task.day }}</p>
  </div>
</template>

<script>
export default {
  name: 'Task',
  props: {
    task: Object
  }
}
</script>

<style scoped>
.task {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}

.task.reminder {
  border-left: 5px solid green;
}

.task h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.fas {
  color: red;
  cursor: pointer;
}

.delete-task {
  border: none;
  background: none;
}
</style>