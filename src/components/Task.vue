<template>
  <div class="tarefa">
    <div class="title" :class="{ 'title-concluida': task.done }">
      <span>{{ task.content }}</span>
    </div>

    <div class="buttons">
      <button @click="$emit('delete')" class="pi pi-times"></button>
      <button @click="$emit('edit')" class="pi pi-pencil"></button>
      <button @click="$emit('completed')" class="pi pi-check"></button>
    </div>
  </div>
</template>

<script>
import './Task.scss'

export default {
  props: {
    task: {
      type: Object,
      required: true
    }
  }
}
</script>
