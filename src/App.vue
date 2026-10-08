<script setup>
import { ref, onMounted } from 'vue'
import ScheduleItem from './components/ScheduleItem.vue'

const schedules = ref([])

const newSchedule = ref({
  subject: '',
  time: '',
  room: ''
})

function addSchedule() {
  if (
    newSchedule.value.subject.trim() === '' ||
    newSchedule.value.time.trim() === '' ||
    newSchedule.value.room.trim() === ''
  ) {
    return
  }

  schedules.value.push({
    id: Date.now(),
    subject: newSchedule.value.subject,
    time: newSchedule.value.time,
    room: newSchedule.value.room
  })

  newSchedule.value = {
    subject: '',
    time: '',
    room: ''
  }

  saveSchedules()
}

function deleteSchedule(id) {
  schedules.value = schedules.value.filter(
    schedule => schedule.id !== id
  )

  saveSchedules()
}

function updateSchedule(updatedSchedule) {
  const index = schedules.value.findIndex(
    schedule => schedule.id === updatedSchedule.id
  )

  if (index !== -1) {
    schedules.value[index] = updatedSchedule
  }

  saveSchedules()
}

function saveSchedules() {
  localStorage.setItem(
    'schedules',
    JSON.stringify(schedules.value)
  )
}

onMounted(() => {
  const savedSchedules = localStorage.getItem('schedules')

  if (savedSchedules) {
    schedules.value = JSON.parse(savedSchedules)
  }
})
</script>

<template>
  <div class="container">

    <h1>My Class Schedule Planner</h1>

    <div class="add-schedule">

      <input
        v-model="newSchedule.subject"
        type="text"
        placeholder="Subject"
      />

      <input
        v-model="newSchedule.time"
        type="text"
        placeholder="Time"
      />

      <input
        v-model="newSchedule.room"
        type="text"
        placeholder="Room"
      />

      <button @click="addSchedule">
        Add
      </button>

    </div>

    <div class="schedule-list">

      <ScheduleItem
        v-for="schedule in schedules"
        :key="schedule.id"
        :schedule="schedule"
        @delete="deleteSchedule"
        @update="updateSchedule"
      />

    </div>

  </div>
</template>

<style>
.container {
  width: 600px;
  margin: 50px auto;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
}

.add-schedule {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.add-schedule input {
  flex: 1;
  padding: 10px;
}

button {
  padding: 10px 15px;
  cursor: pointer;
}

.schedule-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
</style>