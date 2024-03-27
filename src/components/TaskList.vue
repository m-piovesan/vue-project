<template>
  <div class="lista">
    <div class="listagem1">
      <h2>Lista de Tarefas</h2>
      <!-- v-model vincula o valor do input à variável newTask -->
      <input
        type="text"
        v-model="newTask.content"
        @keyup.enter="addTask"
        placeholder="Adicionar nova tarefa"
      />

      <input type="text" placeholder="Buscar tarefas" v-model="searchQuery" />
    </div>

    <div class="listagem">
      <ul>
        <li v-for="(task, index) in filteredTasks" :key="index">
          <Task
            :task="task"
            @delete="deleteTask(index)"
            @edit="editTask(index)"
            @completed="markAsDone(index)"
          />
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import Task from './Task.vue'
import './TaskList.scss'

export default {
  components: {
    Task
  },
  data() {
    return {
      tasks: [],
      newTask: {
        content: '',
        done: false
      },
      searchQuery: '' // Variável para armazenar a entrada de busca do usuário
    }
  },
  mounted() {
    this.loadTasksFromLocalStorage() // Carrega as tarefas ao inicializar o componente
    this.fetchTasks()
  },
  computed: {
    filteredTasks() {
      return this.tasks.filter((task) => {
        return task.content.toLowerCase().includes(this.searchQuery.toLowerCase())
      })
    }
  },
  methods: {
    addTask() {
      // trim() remove espaços em branco do início e do fim da string
      if (this.newTask.content !== '') {
        this.newTask.content = this.newTask.content.trim()
        this.tasks.push(this.newTask)

        this.newTask = { content: '', done: false }
        this.saveTasksToLocalStorage()
      }
    },
    deleteTask(index) {
      // splice() remove um elemento do array na posição index
      this.tasks.splice(index, 1) // remove 1 elemento a partir da posição index
      this.saveTasksToLocalStorage()
    },
    editTask(index) {
      const editedTask = prompt('Editar tarefa', this.tasks[index].content)

      if (editedTask !== null) {
        this.tasks[index].content = editedTask
        this.saveTasksToLocalStorage()
      }
    },
    markAsDone(index) {
      this.tasks[index].done = !this.tasks[index].done
      this.saveTasksToLocalStorage()
    },
    saveTasksToLocalStorage() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },
    loadTasksFromLocalStorage() {
      const savedTasks = localStorage.getItem('tasks')

      if (savedTasks) {
        this.tasks = JSON.parse(savedTasks) // Carrega as tarefas do localStorage
      }
    },
    async fetchTasks() {
      const response = await fetch('https://api.github.com/users/m-piovesan')
      const tasks = await response.json()

      console.log(tasks)
    }
  }
}
</script>
