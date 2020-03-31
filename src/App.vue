<template>
  <div id="app" class="container">
    <img src="./assets/logo.png" class="logo" />
    <h3 class="header">Vue.js Wishlist</h3>
    <todo-list></todo-list>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld";
import TodoList from "./components/TodoList.vue";

export default {
  name: "App",
  components: {
    HelloWorld,
    TodoList
  }
};
</script>

<style>
* {
  box-sizing: border-box;
}

.container {
  max-width: 480px;
  margin: 0 auto;
}
#app {
  font-family: Karla, sans-serif;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
}

.logo {
  display: block;
  margin: 20px auto;
  height: 75px;
}
.header {
  text-align: center;
}
</style>
