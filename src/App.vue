<template>
  <div id="app">
    <Todo/>
  </div>
</template>

<script> 
import Todo from './components/Todo.vue'

export default {
  name: 'App',
  components: {
    Todo,
  }
}
</script>

<style>

  body {
    margin: 0;
    padding: 0;
    width: 100%;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: flex-start;
    background: linear-gradient(to bottom right, #834d9b, #d04ed6); 
    /* background: linear-gradient(to bottom right, #7F00FF, #E100FF);  */
  }

#app {
  
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale; 
  color: #2c3e50;  
  margin-top: 20px;
  width: 100%;
  max-width: 768px;
}
</style>
