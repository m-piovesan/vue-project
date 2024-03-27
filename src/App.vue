<template>
  <div id="app">
    <AHeader />
    <div class="container">
      <TaskList />
    </div>
  </div>
</template>

<script>
import TaskList from './components/TaskList.vue'
import AHeader from './components/AHeader.vue'
import './styles/global.scss'
import 'primeicons/primeicons.css'

export default {
  components: {
    TaskList,
    AHeader
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  display: flex;
  flex-direction: column;
}
</style>
