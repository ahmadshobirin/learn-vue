<template>
  <div id="app" class="container">
    <img src="./assets/gradient.png" class="logo">
    <TodoApp/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import TodoApp from './components/TodoApp'

export default {
  name: 'App',
  components: {
    HelloWorld,
    TodoApp
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
}

* {
  box-sizing: border-box;
}
.container {
  max-width: 600px;
  margin: 0 auto;
}

.logo{
  display: block;
  margin: 20px auto;
  height: 75px;
}
</style>
