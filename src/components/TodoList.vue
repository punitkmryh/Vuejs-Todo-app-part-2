<template>
  <div>
    <div class="input__div">
      <div class="input__wrapper">
        <input
          type="text"
          placeholder="Type here...what's need to be done!!"
          v-model="newtodo"
          @keyup.enter="addTodo"
        />
      </div>
      <div class="border"></div>
    </div>

    <transition-group
      name="fade"
      enter-active-class="animated fadeInUp"
      leave-active-class="animated fadeOutDown"
    >
      <div v-for="(todo, index) in todosFiltered" :key="todo.id" class="todo-item">
        <div class="todo-item-left">
          <label class="material-checkbox">
            <input type="checkbox" v-model="todo.completed" />
            <span></span>
          </label>
          <div
            v-if="!todo.editing"
            @dblclick="editTodo(todo)"
            class="todo-item-label"
            :class="{ completed: todo.completed }"
          >{{ todo.title }}</div>
          <!-- shows if not editing -->
          <div class="input__div" v-else>
            <div class="input__wrapper">
              <input
                type="text"
                v-model="todo.title"
                @keyup.enter="doneEditing(todo)"
                @keyup.esc="cancelEditing(todo)"
                @blur="doneEditing(todo)"
                style="margin-right: 10px;"
                v-focus
              />
            </div>
            <div class="border"></div>
          </div>
        </div>
        <div class="remove-item" @click="removeTodo(index)">&times;</div>
        <!-- gives Cross (x) mark -->
      </div>
    </transition-group>

    <div class="extra-container">
      <div>
        <label class="material-checkbox">
          <input type="checkbox" :checked="anyRemaining" @change="checkAllTodos" style="margin" />
          <span></span> Check all
        </label>
      </div>
      <div>
        <footer>
          <span>{{ remaining }} items left.</span>
        </footer>
      </div>
    </div>

    <div class="extra-container">
      <div>
        <button :class="{ active: filter == 'all' }" @click="filter = 'all'">All</button>
        <button :class="{ active: filter == 'active' }" @click="filter = 'active'">Active</button>
        <button :class="{ active: filter == 'completed' }" @click="filter = 'completed'">Completed</button>
      </div>
      <button v-if="showClearCompletedButton" @click="clearCompleted">Clear Completed</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "todo-list",
  data() {
    return {
      newtodo: "",
      idForTodo: 4,
      beforeEditCache: "",
      filter: "all",
      todolists: [
        {
          id: 1,
          title: "Finish building Todo-app",
          completed: false,
          editing: false
        },
        {
          id: 2,
          title: "Push Code to Github",
          completed: false,
          editing: false
        },
        {
          id: 3,
          title: "Lunch Todo-app on Heroku",
          completed: false,
          editing: false
        }
      ]
    };
  },
  computed: {
    remaining() {
      return this.todolists.filter(todo => !todo.completed).length;
    },
    anyRemaining() {
      return this.remaining == 0;
    },
    todosFiltered() {
      if (this.filter === "all") {
        return this.todolists;
      } else if (this.filter === "active") {
        return this.todolists.filter(todo => !todo.completed);
      } else if (this.filter === "completed") {
        return this.todolists.filter(todo => todo.completed);
      } else {
        return this.todolists;
      }
    },
    showClearCompletedButton() {
      return this.todolists.filter(todo => todo.completed).length > 0;
    }
  },
  directives: {
    focus: {
      inserted: function(el) {
        el.focus();
      }
    }
  },
  methods: {
    addTodo() {
      if (this.newtodo.trim().length == 0) {
        return 0; //trime() removes whitespaces
      }
      this.todolists.push({
        id: this.idForTodo,
        title: this.newtodo,
        completed: false
      });

      this.newtodo = "";
      this.idForTodo++;
    },
    removeTodo(index) {
      this.todolists.splice(index, 1);
    },
    editTodo(todo) {
      this.beforeEditCache = todo.title;
      todo.editing = true;
    },
    cancelEditing(todo) {
      todo.title = this.beforeEditCache;
      todo.editing = false;
    },
    doneEditing(todo) {
      if (todo.title.trim().length == "") {
        todo.title = this.beforeEditCache; //trime() removes whitespaces
      }
      todo.editing = false;
    },
    checkAllTodos() {
      this.todolists.forEach(todo => (todo.completed = event.target.checked));
    },
    clearCompleted() {
      this.todolists = this.todolists.filter(todo => !todo.completed);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
@import url(https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.2/animate.min.css);
.todo-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 15px;
  margin-bottom: 0px;
  &:focus {
    outline: 0;
  }
}
footer {
  padding: 8px 15px;
  background: #76dbae;
  border-radius: 3px;
}

.todo-item {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  animation-duration: 0.4s;
}
.remove-item {
  cursor: pointer;
  margin-left: 14px;
  &:hover {
    color: black;
  }
}
.todo-item-left {
  display: flex;
  align-items: center;
}
.todo-item-label {
  padding: 10px;
  border: 1px solid white;
  margin-left: 12px;
}
.todo-item-edit {
  font-size: 17px;
  color: #2c3e50;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc; //override defaults
  font-family: inherit;
  &:focus {
    outline: none;
  }
}
.completed {
  text-decoration: line-through;
  color: #42b983;
}
.extra-container {
  font-family: inherit;
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 16px;
  border-top: 1px solid lightgrey;
  padding-top: 14px;
  margin-bottom: 14px;
}
.name-container {
  margin-bottom: 16px;
}
button {
  font-size: 14px;
  background-color: white;
  font-family: inherit;
  appearance: flex;
  padding: 4px;
  &:hover {
    background: #42b983;
  }
  &:focus {
    outline: none;
  }
}
.active {
  background: #42b983;
}
// CSS Transitions
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
.todo-list {
  padding: 8px 0;
}

.todo-list .list:hover {
  background: #f7fcfa;
}

.todo-list .list {
  display: flex;
  align-items: center;
  padding: 0 15px;
}

.todo-list .list .text {
  padding: 0 8px;
  height: 35px;
  line-height: 35px;
  margin: 6px 0;
  cursor: default;
  flex: 1;
}

.todo-list .list .text.completed {
  text-decoration: line-through;
}

.input__div {
  margin: 6px 0;
  position: relative;
  border: 1px solid #e4f5ef;
  flex: 1;
}

.input__div:focus-within .input__wrapper {
  background: #fff;
}

.input__div .input__wrapper {
  background: #f7fcfa;
  padding: 0 15px;
  transition: background 0.3s;
}

.input__div .input__wrapper input {
  height: 40px;
  background: 0 0;
  border: none;
  color: #2c3e50;
  display: block;
  font-family: inherit;
  font-size: 16px;
  line-height: 28px;
  outline: 0;
  padding: 0;
  position: relative;
  width: 100%;
  z-index: 1;
}

.input__div .border {
  background: #42b983;
  transition: all 0.18s;
  bottom: -1px;
  height: 2px;
  left: 30%;
  opacity: 0;
  pointer-events: none;
  position: absolute;
  right: 30%;
}

.input__div:focus-within .border {
  left: 0;
  right: 0;
  opacity: 1;
}

.material-checkbox {
  position: relative;
  display: inline-block;
  color: rgba(0, 0, 0, 0.87);
  cursor: pointer;
  font-size: 14px;
  line-height: 18px;
}

.material-checkbox > input {
  appearance: none;
  -moz-appearance: none;
  -webkit-appearance: none;
  position: absolute;
  z-index: -1;
  left: -5px;
  top: -5px;
  display: block;
  margin: 0;
  border-radius: 50%;
  width: 18px;
  height: 18px;
  background-color: rgba(0, 0, 0, 0.42);
  outline: none;
  opacity: 0;
  transform: scale(1);
  -ms-transform: scale(0); /* Graceful degradation for IE */
  transition: opacity 0.5s, transform 0.5s;
}

.material-checkbox > input:checked {
  background-color: #2196f3;
}

.material-checkbox > input:disabled {
  opacity: 0;
}

.material-checkbox > input:disabled + span {
  cursor: initial;
}

.material-checkbox > span::before {
  content: "";
  display: inline-block;
  margin-right: 4px;
  border: solid 2px rgba(0, 0, 0, 0.42);
  border-radius: 2px;
  width: 14px;
  height: 14px;
  vertical-align: -4px;
  transition: border-color 0.5s, background-color 0.5s;
}

.material-checkbox > input:checked + span::before {
  border-color: #41b883;
  background-color: #41b883;
}

.material-checkbox > input:active + span::before {
  border-color: #41b883;
}

.material-checkbox > input:checked:active + span::before {
  border-color: transparent;
  background-color: rgba(0, 0, 0, 0.42);
}

.material-checkbox > span::after {
  content: "";
  display: inline-block;
  position: absolute;
  top: 0;
  left: 0;
  width: 5px;
  height: 10px;
  border: solid 2px transparent;
  border-left: none;
  border-top: none;
  transform: translate(5.5px, 1px) rotate(45deg);
  -ms-transform: translate(5.5px, 2px) rotate(45deg);
}

.material-checkbox > input:checked + span::after {
  border-color: #fff;
}
</style>
