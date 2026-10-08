<script setup>
import { ref } from 'vue'

const props = defineProps({
  schedule: Object
})

const emit = defineEmits([
  'delete',
  'update'
])

const isEditing = ref(false)

const editedSubject = ref(props.schedule.subject)
const editedTime = ref(props.schedule.time)
const editedRoom = ref(props.schedule.room)

function updateSchedule() {
  if (
    editedSubject.value.trim() === '' ||
    editedTime.value.trim() === '' ||
    editedRoom.value.trim() === ''
  ) {
    return
  }

  emit('update', {
    ...props.schedule,
    subject: editedSubject.value,
    time: editedTime.value,
    room: editedRoom.value
  })

  isEditing.value = false
}

function deleteSchedule() {
  emit('delete', props.schedule.id)
}

function startEditing() {
  editedSubject.value = props.schedule.subject
  editedTime.value = props.schedule.time
  editedRoom.value = props.schedule.room

  isEditing.value = true
}
</script>

<template>

  <div class="schedule-item">

    <!-- Normal Mode -->
    <div v-if="!isEditing">

      <h3>{{ schedule.subject }}</h3>

      <p>
        <strong>Time:</strong>
        {{ schedule.time }}
      </p>

      <p>
        <strong>Room:</strong>
        {{ schedule.room }}
      </p>

      <button @click="startEditing">
        Edit
      </button>

      <button @click="deleteSchedule">
        Delete
      </button>

    </div>

    <!-- Edit Mode -->
    <div v-else>

      <input
        v-model="editedSubject"
        type="text"
        placeholder="Subject"
      />

      <input
        v-model="editedTime"
        type="text"
        placeholder="Time"
      />

      <input
        v-model="editedRoom"
        type="text"
        placeholder="Room"
      />

      <button @click="updateSchedule">
        Save
      </button>

      <button @click="isEditing = false">
        Cancel
      </button>

    </div>

  </div>

</template>

<style scoped>

.schedule-item {
  padding: 15px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

.schedule-item h3 {
  margin: 0 0 10px;
}

.schedule-item p {
  margin: 5px 0;
}

.schedule-item input {
  padding: 8px;
  margin: 5px;
}

button {
  margin-left: 5px;
}

</style>