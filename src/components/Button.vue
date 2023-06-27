<template>
  <button 
      @click="toggleAddTask()"
      :style="{background: color}" 
      class="btn">
  {{ text }}
  </button>

</template>

<script>
export default {
  name: 'Button',
  props: {
      text: String,
      color: String,
  },
  methods: {
      toggleAddTask() {
          this.$emit('btn-click')
      }
  }
}
</script>