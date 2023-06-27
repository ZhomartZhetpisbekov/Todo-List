<template>
  <div :key="task.id" v-for="task in tasks">
      <Task @open-task-editor="$emit('open-task-editor', task.id)"
          @delete-task="$emit('delete-task', task.id)" 
          :task="task" />
  </div>
</template>

<script>
import Task from './Task.vue'

export default {
  name: 'Tasks',
  data() {
      return {
      
      }
  },
  props: {
      tasks: Array,
  },
  components: {
      Task
  },
  emits: ['delete-task', 'open-task-editor'],
}
</script>