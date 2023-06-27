<template>
  <form @submit="onSubmit" class="add-form">
    <div class="form-control">
      <label>Task</label>
      <input
        type="text"
        v-model="text"
        name="text"
        :placeholder="changeTask ? taskToEdit.text : 'Add Task'"
      />
    </div>

    <!-- <div class="form-control">
        <label>Day & Time</label>
        <input
          v-model="day"
          type="text"
          name="day"
          :placeholder="changeTask ? taskToEdit.day : 'Add Day & Time'"
        />
      </div> -->

    <div class="form-control">
      <label>Day & Time</label>
      <input
        v-model="datetime"
        type="datetime-local"
        name="datetime"
        min="2022-01-01T00:00"
        max="2022-12-21T23:59"
      />
    </div>

    <div class="form-control form-control-check">
      <label>Set Reminder</label>
      <input v-model="reminder" type="checkbox" name="reminder" />
    </div>

    <input type="submit" :value="changeTask ? 'Edit Task' : 'Save Task'" class="btn btn-block" />
  </form>
</template>

<script>
export default {
  name: 'AddTask',
  props: {
    changeTask: Boolean,
    taskToEdit: Object
  },
  data() {
    return {
      text: '',
      reminder: false,
      datetime: ''
    }
  },
  methods: {
    onSubmit(e) {
      e.preventDefault()

      if (!this.text) {
        alert('Please add a task!')
        return
      }

      const date = new Date(this.datetime)
      const seconds = date.getTime() / 1000
      const month = date.getMonth() + 1
      const day = date.getDate()
      const hours = date.getHours()
      const mins = date.getMinutes()

      if (!this.changeTask) {
        const newTask = {
          // id: Math.floor(Math.random() * 100000),
          text: this.text,
          dayText: this.getMonthName(month) + ' ' + day + ' at ' + hours + ':' + mins,
          reminder: this.reminder,
          datetime: this.datetime,
          secs: seconds
        }

        this.$emit('add-task', newTask)
      } else {
        const newTask = {
          id: this.taskToEdit.id,
          text: this.text,
          dayText: this.getMonthName(month) + ' ' + day + ' at ' + hours + ':' + mins,
          reminder: this.reminder,
          datetime: this.datetime,
          secs: seconds
        }

        this.$emit('edit-task', newTask)
      }

      ;(this.text = ''), (this.reminder = false)
      this.datetime = ''
    },

    getMonthName(month) {
      let monthName = ''
      switch (month) {
        case 1:
          monthName = 'January'
          break
        case 2:
          monthName = 'February'
          break
        case 3:
          monthName = 'March'
          break
        case 4:
          monthName = 'April'
          break
        case 5:
          monthName = 'May'
          break
        case 6:
          monthName = 'June'
          break
        case 7:
          monthName = 'July'
          break
        case 8:
          monthName = 'August'
          break
        case 9:
          monthName = 'September'
          break
        case 10:
          monthName = 'October'
          break
        case 11:
          monthName = 'November'
          break
        case 12:
          monthName = 'December'
          break
      }
      return monthName
    }
  }
}
</script>

<style scoped>
.add-form {
  margin-bottom: 40px;
}

.form-control {
  margin: 20px 0;
}

.form-control label {
  display: block;
}

.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}

.form-control-check {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.form-control-check label {
  flex: 1;
}

.form-control-check input {
  flex: 2;
  height: 20px;
}
</style>
