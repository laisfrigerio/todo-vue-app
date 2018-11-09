<template>
  <div id="app" class="container">
    <img src="./assets/logo.png" class="logo">
    <todo-list></todo-list>
  </div>
</template>

<script>

import TodoList from './components/TodoList'

export default {
  name: 'App',
  components: {
    TodoList,
  }
}

</script>

<style>
  * {
    box-sizing:  border-box;
  }

  .container {
    max-width: 600px;
    margin:  0 auto;
  }

  .logo {
    display: block;
    max-height: 75px;
    margin: 20px auto;
  }


#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*text-align: center;*/
  color: #2c3e50;
  margin-top: 60px;
  font-size: 18px;
}
</style>
