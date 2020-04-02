<template>
  <div id="app" class="container">
    <img src="./assets/logo.png" class="logo" />
    <h3 class="header">Vue.js Wishlist</h3>
    <vue-typer
      text="Hi🙋🏻 Users! Welcome to Wishlist!"
      :repeat="Infinity"
      :shuffle="false"
      initial-action="erasing"
      :pre-type-delay="70"
      :type-delay="70"
      :pre-erase-delay="2000"
      :erase-delay="250"
      erase-style="backspace"
      :erase-on-complete="false"
      caret-animation="expand"
    ></vue-typer>
    <todo-list></todo-list>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld";
import TodoList from "./components/TodoList.vue";
import { VueTyper } from "vue-typer";


export default {
  name: "App",
  components: {
    HelloWorld,
    TodoList,
    VueTyper
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
